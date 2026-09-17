---
name: tax-accounting
description: Tax accounting, fiscal optimization, IRPF, RETA brackets, corporate income tax (IS), VAT/OSS, deductible expenses, withholding calculations, and financial strategy for freelancers, startups, and companies in Spain and the EU.
license: MIT
compatibility: opencode
metadata:
  domain: tax-accounting
  jurisdiction: Spain & European Union
  standards: LIRPF, LIS, LIVA, RETA RD-ley 13/2022, Ley de Startups 28/2022, OSS
---

# Tax Accounting & Fiscal Optimization (Spain & European Union)

Senior tax engineering, bookkeeping, and mathematical fiscal optimization framework for freelancers (*autónomos*), startups, and corporations operating in Spain and the European Union.

This playbook provides surgical, mathematically precise calculations and 100% legal tax-shielding strategies to optimize cash flow, maximize lawful deductions, and eliminate any risk of audit, penalties, or surcharges from the **Agencia Estatal de Administración Tributaria (AEAT)** or the **Tesorería General de la Seguridad Social (TGSS)**.

---

## 1. Personal Income Tax (IRPF) & Professional Withholding Architecture

The Spanish Personal Income Tax (*Impuesto sobre la Renta de las Personas Físicas - LIRPF / Ley 35/2006*) divides taxable income into two distinct schedules:

### 1.1 General Tax Base (*Base Imponible General*) vs. Savings Tax Base (*Base del Ahorro*)
1. **General Tax Base** (Earned Income, Professional Economic Activities, Real Estate Rentals):
   - Progressive tax scale combining the **State Scale** (*Escala Estatal*) and the **Autonomous Community Scale** (*Escala Autonómica*).
   - Aggregate progressive marginal rates:
     - Up to 12,450€: **19%** (9.5% State + 9.5% Regional default)
     - 12,450€ - 20,200€: **24%** (12% + 12%)
     - 20,200€ - 35,200€: **30%** (15% + 15%)
     - 35,200€ - 60,000€: **37%** (18.5% + 18.5%)
     - 60,000€ - 300,000€: **45%** (22.5% + 22.5%)
     - Above 300,000€: **47%** (24.5% + 22.5% or higher depending on Autonomous Community)
2. **Savings Tax Base** (Dividends, Capital Gains from share/crypto sales, Interest):
   - Up to 6,000€: **19%**
   - 6,000€ - 50,000€: **21%**
   - 50,000€ - 200,000€: **23%**
   - 200,000€ - 300,000€: **27%**
   - Above 300,000€: **28%**

### 1.2 Professional Invoicing Withholdings (*Retenciones en Factura*)
- **General Rate**: **15%** on professional invoices issued to Spanish businesses or freelancers (B2B).
- **Reduced Rate for New Freelancers**: **7%** during the year of registration in the IAE and the following two fiscal years, provided the professional was not registered as self-employed in the preceding 12 months.
- **Invoices to B2C (End Consumers) or Foreign Clients**: **0% withholding** (withholding applies only when the client is a Spanish tax-registered withholding agent).

---

## 2. Freelancers (Autónomos): Net Yields, RETA Tiers & Quarterly Filings

### 2.1 Net Yield Calculation (*Rendimiento Neto de Actividad Económica*)
In the Direct Estimation method (*Estimación Directa Simplificada*):
$$\text{Rendimiento Neto Previo} = \text{Ingresos Íntegros Computables} - \text{Gastos Deducibles Justificados}$$
$$\text{Gastos de Difícil Justificación} = \min(5\% \times \text{Rendimiento Neto Previo},\; 2.000€)$$
*(Note: Temporary increase to 7% applies during specific economic recovery fiscal years as enacted by budget laws).*
$$\text{Rendimiento Neto Final} = \text{Rendimiento Neto Previo} - \text{Gastos de Difícil Justificación}$$

### 2.2 RETA Quotation by Real Income (RD-ley 13/2022)
Contributions are calculated based on the monthly average of projected net yields:
$$\text{Rendimiento Neto Mensual} = \frac{\text{Rendimiento Neto Anual} + \text{Cuotas RETA pagadas}}{12}$$
- The monthly yield maps into 15 statutory income brackets with a minimum and maximum contribution base.
- **Flat Rate (Tarifa Plana)**: **80 euros/month** during the first 12 months for new registrations (extendable for 12 additional months if net yields remain below the national minimum wage - SMI).

### 2.3 Mandatory Quarterly Tax Filings
- **Modelo 130 (Quarterly IRPF Fractional Payment)**:
  - **20%** of cumulative net yield from January 1st to the end of the quarter, minus previous quarters' payments and withholding taxes already suffered.
  - *Exemption*: Freelancers whose professional income was subject to withholding on at least **70%** of total turnover in the previous fiscal year are exempt from filing Modelo 130.
- **Modelo 303 (Quarterly VAT / IVA)**:
  - Output VAT collected (*IVA Repercutido*) - Input VAT paid on business expenses (*IVA Soportado Deducible*).
  - Filing deadlines: April 20 (Q1), July 20 (Q2), October 20 (Q3), January 30 (Q4).
- **Modelo 111**: Quarterly settlement of withholdings practiced on employees, contractors, and professionals.
- **Modelo 115**: Quarterly settlement of withholdings practiced on leased commercial properties (19%).

---

## 3. Corporate Income Tax (Impuesto sobre Sociedades - IS) & Startups

### 3.1 Corporate Tax Rates (Ley 27/2014 - LIS)
- **Standard Corporate Rate**: **25%** on Net Taxable Profit (*Base Imponible*).
- **Reduced Rate for Newly Created Companies**: **15%** for the first tax period in which the taxable base is positive and the immediately following tax period.
- **Certified Startup Rate (Ley 28/2022 de Startups)**: **15%** for the first tax period with a positive taxable base and the subsequent 3 years, provided ENISA certification is active.

### 3.2 Key Corporate Tax Reliefs & Shields
1. **Capitalization Reserve (*Reserva de Capitalización* - Art. 25 LIS)**:
   - 10% (or up to 15% under updated budget provisions) reduction in the taxable base corresponding to the increase in net equity, retained in an unavailable reserve for 5 years.
2. **Leveling Reserve (*Reserva de Nivelación* - Art. 105 LIS)**:
   - For small companies (*ERD - Entidades de Reducida Dimensión*): Reduction of up to 10% of the taxable base (maximum 1,000,000€) against future tax losses over the next 5 years.
3. **R&D and Technological Innovation Deductions (*Deducciones por I+D+i* - Art. 35 LIS)**:
   - **Research & Development (I+D)**: 25% base deduction of expenses (plus 17% for dedicated staff costs; and up to 42% for excess expenses over previous 2-year average).
   - **Technological Innovation (IT)**: 12% deduction on software development, cloud architecture prototyping, and UI/UX engineering. Monetizable via cash-back refund if tax quota is insufficient.
4. **Patent Box (Art. 23 LIS)**:
   - 60% tax exemption on net income derived from the licensing or exploitation of qualifying intangible software assets and patents.

---

## 4. Legitimate Tax Deductions & Bookkeeping Optimization Catalog

Proven, 100% compliant strategies to maximize deductible expenses and reduce effective tax burden:

### 4.1 Home Office & Suministros (Art. 30.2.5ª.b LIRPF)
- When working from home, register the exact percentage of the home's square meters dedicated exclusively to economic activity in **Modelo 036/037** (e.g., 20m² out of 100m² = 20%).
- **Deduction Formula for Utilities (Electricity, Water, Gas, Internet)**:
  $$\text{Gasto Deducible} = \text{Facturas de Suministros} \times \% \text{Espacio Afecto} \times 30\%$$
- *Example*: 300€/month in utilities with 20% home office affectation:
  $$300€ \times 0.20 \times 0.30 = 18€/\text{mes} \implies 216€/\text{año}$$
- **Direct Housing Costs (Property Tax / IBI, Community Fees, Garbage, Mortgage Interest)**: Deductible directly in full proportion to the affected square footage (e.g., $100\% \times \% \text{Espacio Afecto}$).

### 4.2 Freelancer Meals & Subsistence Expenses (*Dietas y Manutención* - Art. 30.2.5ª.c LIRPF)
- Deductible daily allowance when dining in hospitality establishments during working days:
  - **In Spain**: **26.67 €/day** (without overnight stay) or **53.34 €/day** (with overnight stay).
  - **Abroad**: **48.08 €/day** (without overnight stay) or **91.35 €/day** (with overnight stay).
- **Mandatory Evidentiary Triad**:
  1. Expense incurred in a restaurant/hospitality establishment (*hostelería*).
  2. Paid electronically (credit/debit card, corporate card; never cash).
  3. Formal complete invoice (*Factura completa* with NIF) issued to the freelancer.

### 4.3 Private Health Insurance (*Seguro de Salud* - Art. 30.2.5ª.a LIRPF)
- Premiums paid for private health insurance covering the freelancer, their spouse, and dependent children under 25 years old residing in the family home.
- **Limit**: **500 €/year per person** (increased to **1,500 €/year** for family members with recognized disabilities).

### 4.4 Hardware, Software Licenses & Cloud Amortization
- **Full Expense vs. Amortization**:
  - Small assets under **300 euros** per unit (up to an aggregate limit of 25,000€/year for small businesses) can be expensed 100% in the year of acquisition.
  - Assets above 300€ (laptops, servers, workstations): Amortized under official AEAT tables for IT equipment (up to 26% linear annual maximum or accelerated depreciation up to 52% for ERD).
- **SaaS Subscriptions & Cloud Hosting** (AWS, GitHub, Supabase, OpenAI APIs, Figma): 100% deductible as operational software services (*Servicios Exteriores* - Account 629/621).

### 4.5 Founder Compensation Optimization: Salary vs. Dividends
- **Arm's Length Principle (Operaciones Vinculadas - Art. 18 LIS)**:
  - Shareholder-directors holding >25% equity must ensure their salary reflects fair market value (*Valor de Mercado*) for their operational functions (e.g., Lead Developer).
- **Tax-Exempt Flexible Compensation (*Retribución en Especie Exenta* - Art. 42 LIRPF)**:
  - **Restaurant Cards / Cheques Restaurante**: Up to 11 €/day (exempt from IRPF).
  - **Public Transport Pass**: Up to 1,500 €/year.
  - **Childcare Vouchers (*Cheques Guardería*)**: 100% exempt with no monetary cap.
  - **Corporate Health Insurance**: Up to 500 €/year per beneficiary.
  - **Employee Training / Courses**: 100% exempt when directly related to the company's tech/business stack.

---

## 5. Cross-Border VAT, VIES & European E-Commerce

### 5.1 Intra-Community B2B Services (VIES & Inversión del Sujeto Pasivo)
- Register in the *Registro de Operadores Intracomunitarios (ROI)* via Modelo 036.
- Invoicing an EU VAT-registered business: **0% VAT** applied on invoice. Mandatory legend: *"Operación exenta de IVA por inversión del sujeto pasivo con arreglo al artículo 196 de la Directiva 2006/112/CE"*.
- Declare quarterly in **Modelo 349** (Recapitulativa de Operaciones Intracomunitarias).

### 5.2 B2C Digital Services & One-Stop Shop (Ventanilla Única - OSS)
- Under the EU Digital Single Market VAT rules, B2C sales of electronic services (SaaS subscriptions, digital downloads) exceeding an aggregate EU cross-border threshold of **10,000 €/year** must be taxed at the destination country's VAT rate (e.g., 20% France, 19% Germany, 22% Italy).
- Instead of registering in 27 tax authorities, file a single quarterly return in Spain via **Modelo 369 (Régimen de la Unión OSS)**.

---

## 6. Official Tax Authorities & Research Repositories

When validating tax rulings, binding consultations (*Consultas Vinculantes de la DGT*), statutory tables, or official forms, verify against:

1. **Agencia Estatal de Administración Tributaria (AEAT)**:
   - Sede Electrónica: `https://sede.agenciatributaria.gob.es/`
   - Manual Práctico de Renta e IRPF: `https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/irpf.html`
   - Manual Práctico de Sociedades: `https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/sociedades.html`
   - Ventanilla Única OSS (Modelo 369): `https://sede.agenciatributaria.gob.es/Sede/iva/ventanilla-unica-oss.html`
   - Buscador de Consultas Vinculantes (DGT): `https://petete.tributos.hacienda.gob.es/consultas/`
2. **Tesorería General de la Seguridad Social (TGSS)**:
   - Calculadora de Cuotas RETA: `https://portal.seg-social.gob.es/multimedia/calculadora-cuotas/`
   - Portal Import@ss: `https://portal.seg-social.gob.es/`
3. **Boletín Oficial del Estado (BOE)**:
   - Ley 35/2006 del IRPF (LIRPF): `https://www.boe.es/buscar/act.php?id=BOE-A-2006-20764`
   - Ley 27/2014 del Impuesto sobre Sociedades (LIS): `https://www.boe.es/buscar/act.php?id=BOE-A-2014-12328`
   - Ley 37/1992 del IVA (LIVA): `https://www.boe.es/buscar/act.php?id=BOE-A-1992-28740`
   - Real Decreto 1619/2012 (Reglamento de Facturación): `https://www.boe.es/buscar/act.php?id=BOE-A-2012-14696`
4. **Unión Europea (EUR-Lex & Taxation)**:
   - Directiva 2006/112/CE del IVA: `https://eur-lex.europa.eu/legal-content/ES/TXT/?uri=celex%3A32006L0112`
   - Sistema VIES de Validación de NIF-IVA: `https://ec.europa.eu/taxation_customs/vies/`
