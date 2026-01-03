# Домашнє завдання: порівняльний аналіз npm‑пакетів (axios 0.21.1 vs node-fetch 2.6.1)

## 1. Вибір пакетів

- **Функція**: обидва пакети використовуються для HTTP‑запитів у Node.js.
- **Обрані версії**: не останні та з історією CVE.
  - **axios 0.21.1**.
  - **node-fetch 2.6.1**.

## 2. Попередній аналіз (Socket.dev)

- **axios 0.21.1**: Supply Chain Security 99, Vulnerability 70, Quality 100, Maintenance 92, License 100. Ознаки ризику: High CVE, Medium CVE, Network access.
- **node-fetch 2.6.1**: Supply Chain Security 99, Vulnerability 85, Quality 100, Maintenance 85, License 100. Ознаки ризику: High CVE, Network access.

## 3. Граф залежностей (deps.dev)

- **axios 0.21.1**: 1 пряма залежність (follow-redirects), 0 транзитивних.
- **node-fetch 2.6.1**: залежності відсутні.
- **Security Advisories**: axios — 4, node-fetch — 1.

## 4. Оцінка безпекової гігієни (OpenSSF Scorecard)

- **axios**: 5.8/10. Слабкі місця: Token‑Permissions (0/10), CII Best Practices (0/10), Fuzzing (0/10), Vulnerabilities (0/10), Pinned‑Dependencies (3/10).
- **node-fetch**: 5.4/10. Слабкі місця: Maintained (0/10), Token‑Permissions (0/10), Pinned‑Dependencies (0/10), CII Best Practices (0/10), Fuzzing (0/10), SAST (0/10).

## 5. Автоматичний пошук вразливостей (OSV‑Scanner)

Сканування виконано для локального тестового проєкту `sca-compare/`, що містить **axios 0.21.1** та **node-fetch 2.6.1**.

| Пакет | Версія | GHSA / CVE | CVSS (OSV) | Фікс | Опис |
| --- | --- | --- | --- | --- | --- |
| axios | 0.21.1 | GHSA-4hjh-wcwx-xvwj / CVE-2025-58754 | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H | 0.30.2, 1.12.0 | DoS через відсутність перевірки розміру даних. |
| axios | 0.21.1 | GHSA-cph5-m8f7-6c5x / CVE-2021-3749 | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H | 0.21.2 | ReDoS через неефективний regex. |
| axios | 0.21.1 | GHSA-jr5f-v2jv-69x6 / CVE-2025-27152 | — | 0.30.0, 1.8.2 | Можливий SSRF / витік облікових даних. |
| axios | 0.21.1 | GHSA-wf5p-g6vw-rhxx / CVE-2023-45857 | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:N/A:N | 0.28.0, 1.6.0 | CSRF‑уразливість. |
| node-fetch | 2.6.1 | GHSA-r683-j2x4-v87g / CVE-2022-0235 | CVSS:3.0/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H | 2.6.7, 3.1.1 | Пересилання захищених заголовків на недовірені хости. |

## 6. SBOM (Syft)

- **Формат**: CycloneDX JSON.
- **Кількість компонентів**: 5.
- **Файл**: `sbom-task2.json`.

## 7. Результати сканування SBOM (Grype)

| Пакет | Версія | GHSA / CVE | Severity |
| --- | --- | --- | --- |
| axios | 0.21.1 | GHSA-4hjh-wcwx-xvwj / CVE-2025-58754 | High |
| axios | 0.21.1 | GHSA-cph5-m8f7-6c5x / CVE-2021-3749 | High |
| axios | 0.21.1 | GHSA-jr5f-v2jv-69x6 / CVE-2025-27152 | High |
| axios | 0.21.1 | GHSA-wf5p-g6vw-rhxx / CVE-2023-45857 | Medium |
| node-fetch | 2.6.1 | GHSA-r683-j2x4-v87g / CVE-2022-0235 | High |

## 8. Ручний пошук CVE + EPSS (NVD + FIRST EPSS)

| Пакет | CVE | CVSS (NVD) | EPSS | Percentile | Коментар |
| --- | --- | --- | --- | --- | --- |
| axios | CVE-2025-58754 | 7.5 | 0.026% | 6.670% | DoS через відсутність перевірки розміру даних. |
| axios | CVE-2021-3749 | 7.5 | 1.069% | 77.345% | ReDoS через неефективний regex. |
| axios | CVE-2025-27152 | 5.3 | 0.093% | 26.734% | Можливий SSRF / витік облікових даних. |
| axios | CVE-2023-45857 | 6.5 | 0.064% | 20.394% | CSRF‑уразливість. |
| node-fetch | CVE-2022-0235 | 6.1 | 0.303% | 53.303% | Пересилання захищених заголовків на недовірені хости. |

## 9. Порівняльна таблиця

| Критерій | axios 0.21.1 | node-fetch 2.6.1 |
| --- | --- | --- |
| Кількість транзитивних залежностей | 0 (1 пряма залежність) | 0 (немає прямих залежностей) |
| Кількість CVE (для версії) | 4 | 1 |
| Середня CVSS (NVD) | 6.7 | 6.1 |
| Найвища EPSS | 1.069% | 0.303% |
| Рівень гігієни (Scorecard) | 5.8/10 | 5.4/10 |
| Ознаки ризику (Socket.dev) | High/Medium CVE, Network access | High CVE, Network access |
| Ліцензія | MIT | MIT |

## 10. Висновки та рекомендації

- **Безпечніший вибір**: **node-fetch 2.6.1** має менше CVE, нижчу середню CVSS та нижчу максимальну EPSS.
- **Ризики процесної гігієни**: у node-fetch нижчий Scorecard, тому потрібен додатковий контроль у CI/CD.
- **Оновлення**: рекомендується перейти на **node-fetch 2.6.7+** або **3.x**. Для axios — на **1.12.0** або новішу гілку.
- **CI/CD політики**: автоматичне SCA (OSV‑Scanner, Syft, Grype), блокування білду при **CVSS ≥ 7.0** або **EPSS ≥ 1%**, автоматичні оновлення залежностей.

## 11. Артефакти

- `sca-compare/package.json`
- `sca-compare/package-lock.json`
- `osv-report-task2.json`
- `sbom-task2.json`
- `grype-report-task2.json`
