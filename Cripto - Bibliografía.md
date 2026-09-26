---
categories:
  - "[[ITBA.base|ITBA]]"
Tags: []
Created: "2026-09-2300:00"
Materia: "[[Criptografía y Seguridad.base|Criptografía y Seguridad]]"
temas:
  - Bibliografía
  - Katz & Lindell
  - Bishop
  - Anderson
  - Handbook of Applied Cryptography
  - Schneier
  - Pfleeger
  - 24 Deadly Sins
  - Mapa libro ↔ clase
---
# Cripto - Bibliografía

## Resumen

La bibliografía oficial de 72.44 tiene 2 libros **básicos** y 5 **de consulta**. La materia tiene dos mitades (según `Cronograma.pdf`):

- **Hasta el 1er parcial (clases 1–5): criptografía** → libro principal: **Katz & Lindell**, más el cap. 11 de **Bishop** para protocolos.
- **Después del 1er parcial (clases 6–11): seguridad** → libro principal: **Bishop**, con **Anderson** para empresa y protección de datos.

> [!tip] Si tenés que elegir uno solo
> Para **toda la materia**: **Bishop**. Cubre casi todo el 2do parcial y tiene criptografía introductoria.
> Para la **parte de criptografía**: **Katz & Lindell**. Las clases 2–4 siguen su enfoque (experimentos EAV/CPA/CCA, PRF/PRP, definiciones formales).

## Ranking

| # | Libro | Tipo | Para qué sirve |
|---|---|---|---|
| 1 | Bishop — *Computer Security: Art and Science* (2ª ed., 2018) | Básica | Libro principal de la 2ª mitad; cap. 11 para protocolos |
| 2 | Katz & Lindell — *Introduction to Modern Cryptography* (3ª ed., 2020) | Básica | Libro principal de la 1ª mitad (criptografía) |
| 3 | Anderson — *Security Engineering* (2ª ed., 2008) | Consulta | Clases 10–11 (empresa, protección de datos) y casos reales |
| 4 | Howard, LeBlanc, Viega — *24 Deadly Sins of Software Security* (2009) | Consulta | Clase 8 / Guía 9 (vulnerabilidades en código) |
| 5 | Pfleeger & Pfleeger — *Security in Computing* (4ª ed., 2006) | Consulta | Panorama general; alternativa más liviana a Bishop |
| 6 | Menezes, van Oorschot, Vanstone — *Handbook of Applied Cryptography* (1997) | Consulta | Referencia puntual de algoritmos y protocolos criptográficos |
| 7 | Schneier — *Applied Cryptography* (2ª ed., 1996) | Consulta | Valor histórico; desactualizado |

## Libros y contenidos

### Katz & Lindell — *Introduction to Modern Cryptography* (3ª ed.)

Criptografía moderna con definiciones formales y reducciones de seguridad. Es la notación de las teóricas de la 1ª mitad.

| Cap. | Título | Clase |
|---|---|---|
| 1 | Introduction (criptografía clásica, principios de la criptografía moderna) | Clase 1 |
| 2 | Perfectly Secret Encryption (secreto perfecto, OTP, Shannon) | Clase 2 |
| 3 | Private-Key Encryption (seguridad computacional, EAV/CPA, PRG, PRF/PRP, modos) | Clase 2 |
| 4 | Message Authentication Codes (MAC, CBC-MAC) | Clase 3 |
| 5 | CCA-Security and Authenticated Encryption | Clase 3 |
| 6 | Hash Functions and Applications (HMAC, Merkle-Damgård) | Clase 3 |
| 7 | Practical Constructions of Symmetric-Key Primitives (stream ciphers, DES, AES) | Clase 2 |
| 8 | Theoretical Constructions of Symmetric-Key Primitives | — (fuera del recorte) |
| 9 | Number Theory and Cryptographic Hardness Assumptions (el álgebra: grupos, RSA, DLog, CDH/DDH) | Clase 4 |
| 10 | Algorithms for Factoring and Computing Discrete Logarithms | — (profundización) |
| 11 | Key Management and the Public-Key Revolution (KDC, Diffie-Hellman) | Clases 4–5 |
| 12 | Public-Key Encryption (RSA, ElGamal) | Clase 4 |
| 13 | Digital Signature Schemes (firma RSA, DSA, certificados/PKI) | Clases 4–5 |
| 14 | Post-quantum / Quantum-Secure Cryptography | Clase 4 (§ post-cuántico) |
| 15 | Advanced Topics in Public-Key Encryption | — |

**Lo que no cubre bien:** protocolos concretos (Needham-Schroeder, Kerberos, TLS en detalle) y todo lo de seguridad de sistemas de la 2ª mitad.

### Bishop — *Computer Security: Art and Science* (2ª ed.)

Seguridad de sistemas con base formal: políticas, modelos, mecanismos y aseguramiento. Cubre casi toda la 2ª mitad.

| Parte | Caps. | Contenido | Clase |
|---|---|---|---|
| I. Introduction | 1 | Panorama de la seguridad: CIA, amenazas, políticas vs. mecanismos | Clase 1 / 6 |
| II. Foundations | 2–3 | Matriz de control de acceso, resultados de decidibilidad (HRU, take-grant) | Clase 6 |
| III. Policy | 4–9 | Políticas de seguridad; confidencialidad (Bell-LaPadula), integridad (Biba, Clark-Wilson), disponibilidad, híbridas (Chinese Wall, RBAC), no interferencia | Clase 6 |
| IV. Cryptography | 10–13 | 10 criptografía básica, **11 manejo de claves y protocolos** (Needham-Schroeder, Kerberos, certificados), 12 técnicas de cifrado (modos, protocolos de red), **13 autenticación** | Clases 5 y 7 |
| V. Systems | 14–18 | **14 principios de diseño** (Saltzer-Schroeder), 15 representación de identidad, 16 mecanismos de control de acceso (ACL, capabilities), **17 flujo de información**, 18 problema del confinamiento (canales encubiertos) | Clases 6–9 |
| VI. Assurance | 19–22 | Aseguramiento, métodos formales, evaluación de sistemas (Common Criteria) | — (consulta) |
| VII. Special Topics | 23–27 | **23 malware**, **24 análisis de vulnerabilidades**, 25 auditoría, 26 detección de intrusiones, 27 ataques y respuestas | Clases 8–9 |
| VIII. Practicum | 28–31 | Seguridad de red, de sistema, de usuario y de programas (caso de estudio aplicado) | Clase 10 |

### Anderson — *Security Engineering* (2ª ed.)

Ingeniería de seguridad aplicada: casos reales, factores humanos, economía. Poca formalización. **El autor ofrece la 2ª edición gratis en su web** (y la 3ª, de 2020, también está disponible en forma libre).

- Caps. 1–7 (fundamentos): qué es la ingeniería de seguridad, usabilidad y psicología (phishing, contraseñas), protocolos, control de acceso, criptografía, sistemas distribuidos, **economía de la seguridad**. → Clases 6, 7, 10
- Caps. 8–9: seguridad multinivel (Bell-LaPadula en la práctica) y multilateral (Chinese Wall, **privacidad, datos médicos, inferencia**). → Clases 6 y 11
- Caps. 10–24 (aplicaciones): banca, protección física, biometría, tamper resistance, emisiones, API attacks, telecomunicaciones, **ataque y defensa de redes**, DRM, marco legal y político. → Clases 9–11, ejemplos
- Caps. 25–27 (gestión): gestión del desarrollo de sistemas seguros, evaluación y aseguramiento, conclusiones. → **Clase 10 (seguridad en la empresa)**

### Menezes, van Oorschot, Vanstone — *Handbook of Applied Cryptography*

Referencia enciclopédica de algoritmos criptográficos (hasta 1997: no incluye AES ni AEAD). **Los 15 capítulos son descarga gratuita y legal** en el sitio de los autores (cacr.uwaterloo.ca/hac).

| Cap. | Título | Clase |
|---|---|---|
| 1 | Overview of Cryptography | Clase 1 |
| 2–4 | Mathematics Background, Number-Theoretic Reference Problems, Public-Key Parameters | Clase 4 (álgebra) |
| 5–7 | Pseudorandom Bits, Stream Ciphers, Block Ciphers (incluye DES y cifrados clásicos) | Clases 1–2 |
| 8 | Public-Key Encryption | Clase 4 |
| 9 | Hash Functions and Data Integrity (MACs) | Clase 3 |
| 10 | Identification and Entity Authentication (contraseñas, challenge-response, zero-knowledge) | Clases 5 y 7 |
| 11 | Digital Signatures | Clase 4 |
| 12 | Key Establishment Protocols (Needham-Schroeder, Kerberos, DH autenticado, **secret sharing**) | Clase 5 |
| 13 | Key Management Techniques (certificados, PKI) | Clase 5 |
| 14–15 | Efficient Implementation, Patents and Standards | — |

### Schneier — *Applied Cryptography* (2ª ed.)

Obra clásica, con enfoque práctico y código en C. Cuatro partes: **protocolos criptográficos** (básicos, intermedios, avanzados, esotéricos), **técnicas** (largo de clave, manejo de claves, modos), **algoritmos** (DES, cifrados de bloque y de flujo, hash, clave pública, firmas) e **implementaciones reales**.
Está **desactualizado**: es anterior a AES, usa definiciones informales y no cubre AEAD, TLS moderno ni post-cuántico. Útil para historia, cifrados clásicos y el catálogo de protocolos.

### Pfleeger & Pfleeger — *Security in Computing* (4ª ed.)

Libro de texto general y accesible. Temas: criptografía introductoria, **seguridad de programas** (fallas, malware, controles), sistemas operativos y control de acceso, bases de datos, redes, **administración de la seguridad** (planes, análisis de riesgo, políticas) y **aspectos legales, éticos y de privacidad**.
→ Cubre las clases 6–11 en menos profundidad que Bishop. Sirve para una primera lectura o repaso.

### Howard, LeBlanc, Viega — *24 Deadly Sins of Software Security*

Catálogo de 24 errores de programación que generan vulnerabilidades. Para cada uno: cómo se ve, cómo detectarlo y cómo corregirlo. Agrupados en:
- **Web:** SQL injection, XSS, CSRF, problemas de servidor web.
- **Implementación:** buffer overruns, format strings, integer overflows, manejo de excepciones, command injection, race conditions, información en mensajes de error, mínimo privilegio.
- **Criptografía:** contraseñas débiles, números aleatorios débiles, uso incorrecto de criptografía.
- **Red:** tráfico sin proteger, PKI mal usada, confiar en la resolución de nombres.
- **Usabilidad**, actualizaciones inseguras, entre otros.

→ Clase 8 (vulnerabilidades), Guía 9, y el TP de implementación (errores de crypto).

## Mapa rápido clase → lectura

| Clase | Tema | Leer |
|---|---|---|
| 1 | Intro, clásica | Katz 1 · HAC 1 |
| 2 | Cifrado | Katz 2, 3, 7 |
| 3 | MAC y cifrado autenticado | Katz 4, 5, 6 |
| 4 | Asimétrico y firma | Katz 9, 11, 12, 13 |
| 5 | Protocolos | **Bishop 11** · HAC 12–13 · RFC 8446 (TLS 1.3) |
| 6 | Políticas y control de acceso | **Bishop 2, 4–8, 16** · Anderson 4, 8 |
| 7 | Autenticación | **Bishop 13, 15** · Anderson 2 · HAC 10 |
| 8 | Principios de diseño y vulnerabilidades | **Bishop 14, 24** · 24 Deadly Sins |
| 9 | Flujo de información y malware | **Bishop 17, 18, 23** |
| 10 | Seguridad en la empresa | Anderson 7, 25–26 · Bishop 28–31 · Pfleeger (administración) |
| 11 | Protección de datos | Anderson 9, 24 · Pfleeger (legal y privacidad) |

> [!note] Cómo se armó
> Las tablas de contenido de Katz (3ª ed. revisada), Bishop (2ª ed.), Anderson (2ª ed.) y HAC se verificaron contra las páginas de las editoriales o de los autores. Schneier, Pfleeger y 24 Deadly Sins se describen a nivel de partes o grupos, sin números de capítulo. La correspondencia con las clases 6–11 se basa en los títulos del cronograma tentativo: revisarla cuando estén las teóricas.

---

<!-- notas-relacionadas:inicio -->

## Notas relacionadas

**Misma materia (Criptografía y Seguridad)**

- [[Cripto - Resumen Teórico Primer Parcial]] — la teoría de la 1ª mitad, basada en los caps. de Katz del mapa de arriba
- [[Protocolos]] — Clase 5, que cita Bishop cap. 11 y el RFC 8446 como bibliografía
- [[Criptografia y seguridad Clase 2 - Cifrado]] — Clase 2, correspondiente a Katz caps. 2–3
- [[Clase 3 - Criptografia - MACs y modo autenticado]] — Clase 3, correspondiente a Katz caps. 4–6
- [[Clase 4 - Criptografía - Cifrado asimétrico y Firma digital]] — Clase 4, correspondiente a Katz caps. 9 y 11–13
- [[Materia - Criptografía y Seguridad]] — índice de la materia

<!-- notas-relacionadas:fin -->

[[Criptografía y Seguridad.base]]
