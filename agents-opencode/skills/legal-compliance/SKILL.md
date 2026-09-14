---
name: legal-compliance
description: Legal compliance, startup law, corporate registration, tax obligations, software licensing, copyright, trademark registration, GDPR/ePrivacy, and e-commerce regulations in Spain and the European Union.
license: MIT
compatibility: opencode
metadata:
  domain: legal
  jurisdiction: Spain & European Union
  standards: RGPD, LSSI-CE, LPI, Ley de Startups 28/2022, Ley Crea y Crece 18/2022, PSD2, AI Act
---

# Legal Compliance & Startup Law (Spain & European Union)

Senior legal playbook and regulatory compliance framework for startups, software creators, freelancers, and corporations operating in Spain and the European Union.

This skill provides unyielding, precise, and non-complacent legal analysis to prevent sanctions from the **AEPD**, investigations by the **Agencia Tributaria (AEAT)**, disputes with the **Seguridad Social (TGSS)**, or lawsuits regarding **Intellectual Property (IP)** and **Unfair Competition**.

---

## 1. Startups & Emerging Companies Framework (Ley 28/2022)

The Spanish Startup Law (*Ley 28/2022 de fomento del ecosistema de las empresas emergentes*) introduces specific tax and administrative incentives for qualified innovative companies.

### 1.1 Requirements for Startup Certification (ENISA)
To access special incentives, a company must be certified by **ENISA** and meet the following criteria:
1. **Age**: Maximum 5 years since incorporation (7 years for biotechnology, energy, industrial, or strategic tech sectors).
2. **Independence**: Not listed on a regulated stock market and not formed via merger/spin-off of non-startup entities.
3. **Headquarters**: Registered office, fiscal domicile, and more than 50% of the workforce with employment contracts in Spain.
4. **Turnover**: Annual revenue under 10 million euros and no distribution of dividends.
5. **Innovative Character**: Evaluated by ENISA assessing the scalability of the business model, R&D intensity, team qualifications, and market viability.

### 1.2 Key Tax & Corporate Advantages
- **Corporate Income Tax (Impuesto sobre Sociedades - IS)**: Reduced rate of **15%** (instead of standard 25%) during the first profitable tax period and the subsequent 3 years while maintaining startup status.
- **Tax Deferral (Aplazamiento de Deuda Tributaria)**: Exemption from paying the first two IS installments without requiring financial guarantees or generating late-payment interest.
- **Stock Options Exemption**: Tax exemption threshold increased up to **50,000 euros/year** (up from 12,000€) when granting shares or options to employees. Liquidity events trigger taxation only when shares are sold or after 10 years.
- **Carried Interest**: 50% tax exemption for managers of venture capital funds.
- **Exemption from NIE Requirement for Foreign Investors**: Electronic processing of NIF for non-resident investors without mandatory physical NIE.

---

## 2. Freelancers (Autónomos) & Moonlighting / Dual Status (Pluriactividad)

For developers and founders launching businesses while employed full-time or part-time under a Spanish employment contract (*cuenta ajena*).

### 2.1 The Pluriactivity Regime (Cuenta Ajena + RETA)
Simultaneous employment (Régimen General) and self-employment (RETA - Régimen Especial de Trabajadores Autónomos).

1. **Mandatory Registration**:
   - High registration is required in both the **Agencia Tributaria (Censo IAE - Modelo 036/037)** and the **Seguridad Social (RETA via Import@ss)** BEFORE issuing the first invoice or starting economic activity.
2. **Quota Discounts vs. Quota Refunds**:
   - *Initial Discount Option*: In cases of new pluriactivity, discounts on the RETA minimum base may apply (up to 50% during the first 18 months for full-time employees, 25% during months 19-36). Note: Selecting this option usually excludes the Flat Rate (*Tarifa Plana*).
   - *Automatic Excess Refund (Art. 28 TRLGSS)*: If total Social Security contributions (General Regime employer + employee + RETA) exceed the legal maximum threshold (set annually in the General State Budget), the TGSS automatically reimburses 50% of the excess contribution during the first half of the following fiscal year without requiring a formal claim.
3. **Contribution Based on Actual Net Income (Real Decreto-ley 13/2022)**:
   - Contributions are scaled into 15 tiers based on projected net annual income (*Rendimientos Netos* = Revenue - Deductible Expenses - 7% generic provision).
   - Monthly installments must be regularized at year-end during the annual IRPF declaration.

### 2.2 Critical Labor & Contractual Risks for Employed Developers
- **Duty of Good Faith and Non-Competition (*Art. 21 Estatuto de los Trabajadores*)**:
  - Unfair competition occurs when the side business operates in the exact same economic sector, serves potential clients of the employer, or uses trade secrets/know-how gained from the employment relationship.
  - *Mitigation*: Request written consent/acknowledgement of non-competition from the employer or ensure strict separation of business vertical and target audience.
- **Exclusivity Clauses (*Pacto de Plena Dedicación*)**: Check the employment contract. If an exclusivity bonus is received, side business activities are strictly prohibited unless waived in writing.

---

## 3. Incorporation of Companies & Shareholder Agreements

### 3.1 Fast-Track S.L. Incorporation (Ley Crea y Crece 18/2022 & CIRCE)
- **Minimum Share Capital**: Reduced from 3,000€ to **1 euro**. (Note: Until the legal reserve reaches 3,000€, at least 20% of net profits must be allocated to the legal reserve, and partners remain jointly and severally liable up to 3,000€ in case of liquidation).
- **Electronic CIRCE / DUE Processing**:
  1. Negative name certification from the Central Commercial Registry (*Registro Mercantil Central - RMC*).
  2. Telematics filing via the *Punto de Atención al Emprendedor (PAE)* using the Single Electronic Document (*Documento Único Electrónico - DUE*).
  3. Notarial deed execution using standardized bylaws (*Estatutos Tipo*).
  4. Automatic assignment of provisional NIF and provisional registration in the Commercial Registry (*Registro Mercantil*) within 6 to 48 hours.

### 3.2 Key Clauses in Startup Shareholder Agreements (Pacto de Socios)
Public bylaws are generic; critical founder relationships MUST be governed by a private Shareholder Agreement:
- **Vesting of Shares**: Standard 4-year schedule with a 1-year cliff (25% unlocked at month 12, remaining 75% linear monthly over 36 months).
- **Good Leaver vs. Bad Leaver**:
  - *Good Leaver* (death, permanent disability, unfair dismissal): Retains vested shares; unvested shares repurchased at nominal or fair market value.
  - *Bad Leaver* (voluntary resignation during vesting, gross misconduct, breach of non-competition): Company/founders have call option to repurchase ALL shares (vested and unvested) at nominal value (e.g., 0.01€ per share).
- **Drag-Along (Derecho de Arrastre)**: Majority shareholders can compel minority holders to sell their shares under identical terms upon a bona fide acquisition offer (e.g., >51% or >75% approval).
- **Tag-Along (Derecho de Acompañamiento)**: Protects minority shareholders by allowing them to join the sale if a founding/majority shareholder sells their stake to a third party.
- **Intellectual Property Assignment**: Irrevocable assignment of all code, designs, algorithms, and trade secrets created by founders/contractors directly to the company.

---

## 4. Software Law, Intellectual Property & Licensing

### 4.1 Software Copyright under Spanish Law (Real Decreto Legislativo 1/1996 - LPI)
Under Title VII of the *Ley de Propiedad Intelectual*, computer programs (source code, object code, technical documentation, and user manuals) are protected as literary works.

- **Moral Rights vs. Economic Rights**:
  - *Moral Rights* (paternity, integrity) are non-transferable and inalienable under Spanish law.
  - *Economic Rights* (exploitation, reproduction, distribution, public communication, transformation) can be assigned exclusively or non-exclusively.
- **CRITICAL ALERT - Software Created by Employees (Art. 97.4 LPI)**:
  - *"Unless otherwise agreed, the ownership of the economic rights of a computer program created by a salaried employee in the performance of their duties or following instructions of their employer belongs exclusively to the employer."*
  - **Rules for Side-Projects**:
    1. NEVER use company equipment, laptops, servers, GitHub accounts, or software licenses.
    2. NEVER write code during contracted working hours.
    3. Ensure the project is outside the technological or business scope of the employer's operational domain.

### 4.2 Open Source License Compliance & Contamination
- **Permissive Licenses** (MIT, Apache 2.0, BSD-3-Clause): Permit proprietary commercial use, distribution, and closed-source monetization. Only requirement: retain copyright notices and license text.
- **Strong Copyleft Licenses** (GPL v2, GPL v3, AGPL v3):
  - *GPL Contamination*: If proprietary code statically or dynamically links to a GPL library, the entire derivative work must be released under GPL upon distribution.
  - *AGPL Network Trigger*: Accessing the software over a network (SaaS/cloud) triggers the obligation to make the entire source code available to network users.
  - *Rule*: Never import GPL/AGPL libraries into closed-source commercial SaaS backend/frontend codebases without dual-licensing rights from the copyright owner.

---

## 5. Trademark, Trade Name & Asset Registration

### 5.1 Trademark Protection in Spain & EU
A commercial name or domain registration (.com, .es) does NOT confer trademark protection. Exclusive rights require registration:
1. **National Trademark (Spain - OEPM)**:
   - Governed by *Ley 17/2001 de Marcas*. Valid for 10 years, indefinitely renewable.
   - Protection within Spanish territory.
2. **European Union Trademark (EUTM - EUIPO)**:
   - Single unitary registration covering all 27 EU member states.
3. **Nice Classification for Tech & Software**:
   - **Class 9**: Computer software, mobile applications, downloadable digital goods.
   - **Class 35**: Advertising, business management, online marketplaces, commercial SaaS promotion.
   - **Class 42**: Software as a Service (SaaS), cloud hosting, software development, IT consultancy.

### 5.2 Pre-Registration Due Diligence
Before adopting or publicizing a brand:
- Conduct prior art and identity searches in OEPM (Sitadex / Clinmar) and EUIPO (TMview / eSearch Plus) to detect identical or confusingly similar trademarks in identical/similar Nice classes.
- Prevent opposition proceedings and claims under the *Ley 3/1991 de Competencia Desleal*.

---

## 6. E-Commerce, Subscriptions, Payments & Fiscal Models

### 6.1 Legal Notices & Electronic Commerce (LSSI-CE - Ley 34/2002)
Any website or SaaS with commercial activity or direct/indirect monetization must include visible, permanently accessible legal information:
- **Mandatory Identification (Footer / Legal Notice)**:
  - Commercial and corporate name, fiscal identification number (NIF/CIF).
  - Physical registered address and contact email / phone.
  - Commercial Registry registration details (Tom, Folio, Sheet, Inscription).
- **Terms of Service & Contracting Conditions**:
  - Detailed pricing breakdown (including/excluding VAT and payment processing fees).
  - Delivery timelines, subscription renewal cycles, and cancellation mechanics.

### 6.2 Consumer Protection & Subscription Recurring Billing
- **Right of Withdrawal (Derecho de Desistimiento - 14 Days)**:
  - Under *Real Decreto Legislativo 1/2007*, consumers have 14 natural days to withdraw from digital purchases.
  - *Exception for Digital Content*: Immediate consumption requires the user's prior express consent and acknowledgment that they lose their right of withdrawal once the download/streaming starts (e.g., explicit checkbox during checkout).
- **Anti-Dark Patterns & 1-Click Cancellation**:
  - Under Spanish and EU consumer regulations, cancelling a recurring subscription must be as immediate, accessible, and frictionless as subscribing (no forced phone calls, deceptive multi-step cancellation flows, or hidden buttons).

### 6.3 Payment Processing & European VAT (PSD2 & OSS)
- **PSD2 & Strong Customer Authentication (SCA)**: Online payments must support 3D Secure 2.0 (biometric or two-factor authentication) via compliant gateways (Stripe, Adyen).
- **Intra-Community VAT & VIES**:
  - B2B transactions within the EU: Register in the *Registro de Operadores Intracomunitarios (ROI)* via Modelo 036 to obtain a validated VIES VAT number. Invoices issued to verified EU businesses are exempt from VAT under the reverse charge mechanism (*inversión del sujeto pasivo*).
- **One-Stop Shop (Ventanilla Única - OSS)**:
  - B2C digital services (SaaS, downloads) sold to non-business consumers across EU member states: Apply the VAT rate of the buyer's destination country once total EU cross-border sales exceed 10,000€/year. Quarterly declaration filed through the Spanish Tax Agency using **Modelo 369**.

---

## 7. Data Protection, Privacy & AI Regulations

### 7.1 GDPR (Regulation EU 2016/679) & LOPDGDD (Ley Orgánica 3/2018)
- **Lawful Bases for Processing (Art. 6 RGPD)**: Contract execution, explicit consent, legal obligation, or legitimate interest.
- **Prohibited Consent Practices**: Pre-ticked checkboxes, bundling consent with terms of service, or cookie walls without a free alternative.
- **Mandatory Documentation**:
  - **Register of Processing Activities (RAT - Registro de Actividades de Tratamiento)**: Internal catalog of data categories, purposes, retention periods, and security measures.
  - **Data Processing Agreements (DPA - Contratos de Encargado de Tratamiento)**: Mandatory signed contracts under Art. 28 RGPD with any third-party vendor processing user data (hosting, analytics, CRM, transactional email).
  - **International Data Transfers**: Data hosted in the US must rely on certified frameworks (EU-U.S. Data Privacy Framework) or Standard Contractual Clauses (SCC) with a Transfer Impact Assessment (TIA).

### 7.2 Cookie Compliance (AEPD Guidelines & ePrivacy)
- First-layer cookie banner must offer equal visual prominence to **"Accept All"**, **"Reject Non-Essential"**, and **"Configure Preferences"**.
- Tracking and analytics cookies must remain blocked until explicit consent is granted.

### 7.3 EU Artificial Intelligence Act (Regulation EU 2024/1689)
- **Prohibited Practices**: Social scoring, biometric categorization based on sensitive traits, subliminal manipulation.
- **High-Risk AI Systems**: Subject to conformity assessments, human oversight, logging, and risk mitigation protocols.
- **General Purpose AI (GPAI) & Transparency**: AI-generated content (deepfakes, synthetic text/images) must be clearly labeled and watermarked.

---

## 8. Images, Media, Copyright & Right to Honor

### 8.1 Image Rights & Privacy (Ley Orgánica 1/1982)
- Capturing, reproducing, or commercially exploiting an individual's recognizable likeness requires explicit written consent (*Cesión de Derechos de Imagen*), defining scope, duration, territory, and commercial mediums.
- Unauthorized commercial use in advertising or SaaS marketing constitutes an illegitimate intrusion with strict civil liability.

### 8.2 Photography & Digital Assets (Art. 128 LPI vs Art. 10 LPI)
- **Photographic Works (*Obras Fotográficas*)**: Possess creative originality. Protected for 70 years after the author's death.
- **Simple Photographs (*Meras Fotografías*)**: Plain documentation lacking original artistic expression. Protected for 25 years from the date of capture.
- **Stock & Commercial Licensing**: Always verify and archive the commercial license (Standard vs Extended/Enterprise) to avoid automated copyright infringement claims from rights management firms.

---

## 9. Official Authoritative Repositories & Legal Research URLs

When validating statutes, case law, tax rulings, or official procedures, verify against the following authoritative repositories:

### 9.1 Spanish Legislation & Official Bulletins
- **Boletín Oficial del Estado (BOE)**: `https://www.boe.es/`
  - Ley 28/2022 de Startups: `https://www.boe.es/buscar/act.php?id=BOE-A-2022-21739`
  - Ley 18/2022 Crea y Crece: `https://www.boe.es/buscar/act.php?id=BOE-A-2022-15818`
  - Ley de Propiedad Intelectual (LPI): `https://www.boe.es/buscar/act.php?id=BOE-A-1996-8930`
  - Ley 34/2002 (LSSI-CE): `https://www.boe.es/buscar/act.php?id=BOE-A-2002-13758`
  - Ley Orgánica 3/2018 (LOPDGDD): `https://www.boe.es/buscar/act.php?id=BOE-A-2018-16673`
  - Estatuto de los Trabajadores: `https://www.boe.es/buscar/act.php?id=BOE-A-2015-11430`
  - Ley General de Consumidores: `https://www.boe.es/buscar/act.php?id=BOE-A-2007-20555`

### 9.2 Tax & Social Security Administrations
- **Agencia Tributaria (AEAT)**: `https://sede.agenciatributaria.gob.es/`
  - Registro VIES e IVA Intracomunitario: `https://ec.europa.eu/taxation_customs/vies/`
  - Sistema de Ventanilla Única (OSS): `https://sede.agenciatributaria.gob.es/Sede/iva/ventanilla-unica-oss.html`
- **Seguridad Social (TGSS - Portal Import@ss)**: `https://portal.seg-social.gob.es/`
  - Guía de Pluriactividad y RETA: `https://portal.seg-social.gob.es/multimedia/importass/`

### 9.3 Corporate Incorporation & Innovation Certification
- **Centro de Información y Red de Creación de Empresas (CIRCE)**: `https://circe.serviciostelemaricos.es/`
- **ENISA (Certificación de Empresas Emergentes)**: `https://www.enisa.es/es/certificacion-de-startups`
- **Registro Mercantil Central (RMC)**: `https://www.rmc.es/`

### 9.4 Intellectual Property, Trademarks & Privacy
- **Oficina Española de Patentes y Marcas (OEPM)**: `https://www.oepm.es/`
  - Base de Datos Sitadex / Marcas: `https://consultas2.oepm.es/sitadex-controller/`
- **European Union Intellectual Property Office (EUIPO)**: `https://euipo.europa.eu/`
  - Buscador TMview: `https://www.tmdn.org/tmview/`
- **Agencia Española de Protección de Datos (AEPD)**: `https://www.aepd.es/`
  - Guía sobre el uso de cookies: `https://www.aepd.es/guias/guia-cookies.pdf`
  - Resoluciones y Régimen Sancionador: `https://www.aepd.es/es/resoluciones`

### 9.5 European Union Law & Digital Single Market
- **EUR-Lex (Acceso al Derecho de la Unión Europea)**: `https://eur-lex.europa.eu/`
  - Reglamento General de Protección de Datos (RGPD UE 2016/679): `https://eur-lex.europa.eu/eli/reg/2016/679/oj`
  - Reglamento de Inteligencia Artificial (AI Act UE 2024/1689): `https://eur-lex.europa.eu/eli/reg/2024/1689/oj`
  - Directiva de Servicios de Pago (PSD2 UE 2015/2366): `https://eur-lex.europa.eu/eli/dir/2015/2366/oj`
  - Directiva sobre Derechos de Autor en el Mercado Único Digital (UE 2019/790): `https://eur-lex.europa.eu/eli/dir/2019/790/oj`
