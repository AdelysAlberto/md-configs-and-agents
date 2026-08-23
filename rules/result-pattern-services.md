---
trigger: model_decision
---

# Patrón Result en Servicios API

## 📋 Descripción

El **Patrón Result** es un enfoque de manejo de errores que **evita el uso de excepciones** (`throw/catch`) y en su lugar retorna un objeto que encapsula tanto el éxito como el fallo de una operación.

Este patrón proporciona:
- ✅ **Manejo explícito de errores** - El consumidor decide qué hacer con errores
- ✅ **Sin excepciones no controladas** - No hay sorpresas en runtime
- ✅ **Tipado fuerte** - TypeScript sabe exactamente qué puede retornar
- ✅ **Código más limpio** - Sin bloques try/catch anidados

---

## 🏗️ Estructura del Patrón

### Tipo `TResponseRequest<T>`

```typescript
export type TResponseRequest<T> = {
  data?: T;           // Datos de respuesta cuando la operación es exitosa
  status?: number;    // Código de estado HTTP (200, 400, 500, etc.)
  message?: string;   // Mensaje de error cuando falla la operación
};
```

### ¿Cuándo usar cada campo?

| Campo     | Cuando la operación es **exitosa** | Cuando la operación **falla** |
|-----------|-----------------------------------|-------------------------------|
| `data`    | ✅ Contiene los datos solicitados | ❌ `undefined`                |
| `status`  | ✅ 200, 201, 204, etc.            | ✅ 400, 401, 404, 500, etc.   |
| `message` | ❌ `undefined`                    | ✅ Descripción del error      |

---

## 🔨 Implementación en Servicios

### ❌ Patrón INCORRECTO (lanzar excepciones)

```typescript
// ❌ MAL - Usar throw/try-catch
export const checkKycStatus = async (): Promise<KycCheckResponse> => {
  try {
    const response = await fetchPrivate("GET", url);
    
    if ("message" in response && response.status !== 200) {
      throw new Error(response.message); // ❌ Lanzar excepción
    }
    
    return response.data;
  } catch (error) {
    console.error("Error:", error);
    throw error; // ❌ Re-lanzar excepción
  }
};
```

### ✅ Patrón CORRECTO (usar Result Pattern)

```typescript
// ✅ BIEN - Usar privateRequest que retorna TResponseRequest
import type {TResponseRequest} from "../interfaces/http.interface";
import privateRequest from "../providers/http/request.http";

export const checkKycStatus = async (): Promise<TResponseRequest<KycCheckResponse>> => {
  const url = `${Config.apiOnboarding}/kyc/check`;

  if (__DEV__) {
    console.log("🌐 [KYC API CALL] Fetching KYC status from:", url);
  }

  // privateRequest.get NO lanza excepciones, retorna un objeto Result
  const response = await privateRequest.get<KycCheckResponse>({url});

  if (__DEV__) {
    if (response.data) {
      console.log("✅ [KYC API SUCCESS]:", response.data);
    } else {
      console.log("❌ [KYC API ERROR]:", {
        status: response.status,
        message: response.message,
      });
    }
  }

  return response; // Retornar el objeto Result
};
```

---

## 🎣 Consumo en Custom Hooks (React Query)

Cuando consumes un servicio que implementa el Patrón Result en un hook de React Query, debes **verificar si `data` existe** y lanzar un error para que React Query maneje el estado de error correctamente.

### Patrón recomendado:

```typescript
import {useQuery} from "@tanstack/react-query";
import {checkKycStatus} from "../services/kyc.service";

export const useKycOnboarding = (options = {}) => {
  const query = useQuery({
    queryKey: ["kyc-onboarding", "status"],
    queryFn: async () => {
      const response = await checkKycStatus();

      // ⚠️ IMPORTANTE: Validar el Result
      if (!response.data) {
        const errorMessage = response.message || "Error fetching KYC status";
        
        if (__DEV__) {
          console.error("❌ Error:", {
            status: response.status,
            message: errorMessage,
          });
        }
        
        // Lanzar error para que React Query lo maneje
        throw new Error(errorMessage);
      }

      // Retornar solo los datos exitosos
      return response.data;
    },
    enabled: options.enabled,
    staleTime: 2 * 60 * 1000,
  });

  return {
    data: query.data,              // KycCheckResponse directamente
    isLoading: query.isLoading,
    error: query.error?.message,
    isError: query.isError,
    refetch: query.refetch,
  };
};
```

### ¿Por qué lanzar un error en el hook?

React Query **necesita** que las funciones `queryFn` lancen errores cuando fallan para poder:
- Marcar `isError: true`
- Almacenar el error en `query.error`
- Activar reintentos automáticos
- Mostrar estados de error en la UI

Por eso, aunque el servicio NO lance excepciones, el **hook sí debe lanzarlas** cuando detecta que `response.data` es `undefined`.

---

## 🎨 Consumo en Componentes UI

### Enfoque 1: React Query Hook (Recomendado)

```typescript
const LoginScreen = () => {
  const {data, isLoading, isError, error} = useKycOnboarding();

  if (isLoading) {
    return <LoadingSpinner />;
  }

  if (isError) {
    return <ErrorMessage message={error} />;
  }

  return <KycStatus data={data} />;
};
```

### Enfoque 2: Mutation (Para operaciones POST/PUT/DELETE)

```typescript
import {useMutation} from "@tanstack/react-query";
import {loginService} from "../services/auth/auth.service";

const LoginForm = () => {
  const mutation = useMutation({
    mutationFn: async (credentials: {username: string; password: string}) => {
      const response = await loginService(credentials);

      // Validar Result pattern
      if (!response.data) {
        throw new Error(response.message || "Login failed");
      }

      return response.data;
    },
    onSuccess: (data) => {
      // Guardar token, redirigir, etc.
      console.log("Login successful:", data);
    },
    onError: (error: Error) => {
      // Mostrar mensaje de error
      console.error("Login failed:", error.message);
    },
  });

  const handleSubmit = (username: string, password: string) => {
    mutation.mutate({username, password});
  };

  return (
    <form onSubmit={handleSubmit}>
      {mutation.isError && <p>Error: {mutation.error.message}</p>}
      {mutation.isLoading && <p>Loading...</p>}
      <button type="submit">Login</button>
    </form>
  );
};
```

---

## 📦 Herramientas del Patrón

### `privateRequest` (Recomendado)

**Ubicación**: `app-core/providers/http/request.http.ts`

Esta es la **forma recomendada** de hacer peticiones HTTP porque implementa el Patrón Result automáticamente.

```typescript
import privateRequest from "@/app-core/providers/http/request.http";

// GET request
const response = await privateRequest.get<User>({
  url: `${Config.apiBase}/v1/users/me`,
});

// POST request
const response = await privateRequest.post<CreateUserResponse>({
  url: `${Config.apiBase}/v1/users`,
  body: {name: "John", email: "john@example.com"},
});

// PUT request
const response = await privateRequest.put<UpdateUserResponse>({
  url: `${Config.apiBase}/v1/users/123`,
  body: {name: "John Updated"},
});

// DELETE request
const response = await privateRequest.delete({
  url: `${Config.apiBase}/v1/users/123`,
});
```

### `fetchPrivate` (Uso avanzado)

**Ubicación**: `app-core/providers/http/request.instance.ts`

Esta función es de **más bajo nivel** y lanza excepciones en errores 4xx. Solo úsala si necesitas control manual o en casos especiales.

```typescript
// ⚠️ Uso avanzado - lanza excepciones
import {fetchPrivate} from "@/app-core/providers/http/request.instance";

try {
  const response = await fetchPrivate("GET", url);
  return response.data;
} catch (error) {
  // Manejar error manualmente
}
```

---

## 🚀 Migración desde throw/catch a Result Pattern

### Paso 1: Actualizar el servicio

**Antes:**
```typescript
import {fetchPrivate} from "../providers/http/request.instance";

export const getUserProfile = async (): Promise<UserProfile> => {
  try {
    const response = await fetchPrivate("GET", url);
    return response.data;
  } catch (error) {
    throw error;
  }
};
```

**Después:**
```typescript
import type {TResponseRequest} from "../interfaces/http.interface";
import privateRequest from "../providers/http/request.http";

export const getUserProfile = async (): Promise<TResponseRequest<UserProfile>> => {
  const response = await privateRequest.get<UserProfile>({url});
  return response;
};
```

### Paso 2: Actualizar el hook

**Antes:**
```typescript
const query = useQuery({
  queryKey: ["user-profile"],
  queryFn: async () => {
    const data = await getUserProfile();
    return data;
  },
});
```

**Después:**
```typescript
const query = useQuery({
  queryKey: ["user-profile"],
  queryFn: async () => {
    const response = await getUserProfile();
    
    if (!response.data) {
      throw new Error(response.message || "Failed to fetch user profile");
    }
    
    return response.data;
  },
});
```

---

## ✅ Checklist de Implementación

Al crear un nuevo servicio API, verifica:

- [ ] **Importa `TResponseRequest`** desde `app-core/interfaces/http.interface.ts`
- [ ] **Importa `privateRequest`** desde `app-core/providers/http/request.http.ts`
- [ ] **El servicio retorna `Promise<TResponseRequest<T>>`**
- [ ] **NO usa `throw new Error()`** (sin excepciones)
- [ ] **NO usa bloques `try/catch`**
- [ ] **El hook valida `response.data` antes de retornar**
- [ ] **El hook lanza error si `!response.data`** para React Query
- [ ] **Logs condicionales con `__DEV__`** (no en producción)

---

## 📚 Ejemplos Completos

### Ejemplo 1: Servicio de Autenticación

```typescript
// app-core/services/auth/auth.service.ts
import Config from "@/app-core/constants/envs";
import type {TResponseRequest} from "@/app-core/interfaces/http.interface";
import privateRequest from "@/app-core/providers/http/request.http";

type LoginResponse = {
  accessToken: string;
  refreshToken: string;
  user: {
    id: string;
    email: string;
    isActive: boolean;
  };
};

export const loginService = async ({
  username,
  password,
}: {
  username: string;
  password: string;
}): Promise<TResponseRequest<LoginResponse>> => {
  const response = await privateRequest.post<LoginResponse>({
    url: `${Config.apiBase}/v1/auth/login`,
    body: {username, password},
  });

  return response;
};
```

### Ejemplo 2: Hook personalizado

```typescript
// app-core/hooks/useLogin.ts
import {useMutation} from "@tanstack/react-query";
import {loginService} from "../services/auth/auth.service";
import useAuthStore from "../stores/authStore";

export const useLogin = () => {
  const {setTokens, setUser} = useAuthStore();

  return useMutation({
    mutationFn: async (credentials: {username: string; password: string}) => {
      const response = await loginService(credentials);

      if (!response.data) {
        throw new Error(response.message || "Login failed");
      }

      return response.data;
    },
    onSuccess: (data) => {
      setTokens(data.accessToken, data.refreshToken);
      setUser(data.user);
    },
  });
};
```

### Ejemplo 3: Componente de Login

```typescript
// app/public/index.tsx
import {BaseButton, BaseInput} from "@/app-core/components";
import {useLogin} from "@/app-core/hooks/useLogin";
import {useState} from "react";

export default function LoginScreen() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const login = useLogin();

  const handleLogin = () => {
    login.mutate({username, password});
  };

  return (
    <>
      <BaseInput
        value={username}
        onChangeText={setUsername}
        placeholder="Username"
      />
      <BaseInput
        value={password}
        onChangeText={setPassword}
        placeholder="Password"
        secureTextEntry
      />
      
      {login.isError && (
        <BaseText style={{color: "red"}}>
          {login.error?.message}
        </BaseText>
      )}
      
      <BaseButton
        title={login.isLoading ? "Loading..." : "Login"}
        onPress={handleLogin}
        disabled={login.isLoading}
      />
    </>
  );
}
```

---

## 🎯 Beneficios del Patrón Result

### Comparación: throw/catch vs Result Pattern

| Aspecto                    | `throw/catch`                          | **Result Pattern**                    |
|----------------------------|----------------------------------------|---------------------------------------|
| **Tipado**                 | ❌ Error no está tipado                | ✅ Error está en el tipo de retorno  |
| **Explícito**              | ❌ Implícito (puede sorprender)         | ✅ Explícito (siempre visible)        |
| **Stack Traces**           | ✅ Stack trace completo                | ⚠️ Debe lanzarse en el hook           |
| **Performance**            | ❌ Excepciones son lentas              | ✅ Sin overhead de excepciones        |
| **Manejo de errores**      | ❌ Fácil olvidar try/catch             | ✅ TypeScript obliga a manejar        |
| **Testing**                | ❌ Complicado testear excepciones       | ✅ Fácil verificar `data` o `message` |

---

## 🔍 Debugging

### Verificar respuestas en desarrollo

```typescript
const response = await checkKycStatus();

if (__DEV__) {
  console.log("📦 Full response:", {
    hasData: !!response.data,
    status: response.status,
    message: response.message,
  });
}
```

### Log condicional por tipo de respuesta

```typescript
if (__DEV__) {
  if (response.data) {
    console.log("✅ SUCCESS:", response.data);
  } else {
    console.log("❌ ERROR:", {
      status: response.status,
      message: response.message,
    });
  }
}
```

---

## ⚠️ Errores Comunes

### Error 1: No validar `response.data` en el hook

```typescript
// ❌ MAL - React Query no sabrá que hubo un error
const query = useQuery({
  queryFn: async () => {
    const response = await checkKycStatus();
    return response.data; // ⚠️ Puede ser undefined
  },
});
```

```typescript
// ✅ BIEN - Lanzar error si no hay datos
const query = useQuery({
  queryFn: async () => {
    const response = await checkKycStatus();
    
    if (!response.data) {
      throw new Error(response.message || "Unknown error");
    }
    
    return response.data;
  },
});
```

### Error 2: Usar `fetchPrivate` directamente en servicios

```typescript
// ❌ MAL - fetchPrivate lanza excepciones
import {fetchPrivate} from "../providers/http/request.instance";

export const getUser = async () => {
  const response = await fetchPrivate("GET", url); // Puede lanzar excepción
  return response.data;
};
```

```typescript
// ✅ BIEN - Usar privateRequest que retorna Result
import privateRequest from "../providers/http/request.http";

export const getUser = async (): Promise<TResponseRequest<User>> => {
  const response = await privateRequest.get<User>({url});
  return response;
};
```

### Error 3: Retornar solo `data` en el servicio

```typescript
// ❌ MAL - Pierde información de status y message
export const getUser = async (): Promise<User | undefined> => {
  const response = await privateRequest.get<User>({url});
  return response.data; // ⚠️ No se puede saber por qué falló
};
```

```typescript
// ✅ BIEN - Retornar el objeto Result completo
export const getUser = async (): Promise<TResponseRequest<User>> => {
  const response = await privateRequest.get<User>({url});
  return response; // ✅ Incluye data, status, y message
};
```

---

## 📖 Referencias

- **Tipo Result**: `app-core/interfaces/http.interface.ts`
- **Implementación**: `app-core/providers/http/request.http.ts`
- **Ejemplo de servicio**: `app-core/services/auth/auth.service.ts`
- **Ejemplo de hook**: `app-core/hooks/useKycOnboarding.ts`

---

## 🎓 Resumen

1. **Los servicios NO lanzan excepciones** - retornan `TResponseRequest<T>`
2. **Los hooks SÍ lanzan excepciones** - cuando `!response.data`
3. **Usar `privateRequest`** - NO `fetchPrivate` directamente
4. **Validar `response.data`** - siempre en el hook
5. **Tipado fuerte** - TypeScript te guía en el manejo de errores

El Patrón Result hace el código más **predecible, testeable y mantenible**. 🚀
