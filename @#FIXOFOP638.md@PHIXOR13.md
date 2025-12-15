Perfecto — preparé un patch listo para añadir un archivo SUPPORT_FEEDS.md al repo con las dos URLs de feed (RSS + Atom), tus datos de contacto y una breve nota sobre el Image 1 que adjuntaste. También incluí una sugerencia de uso (cron/script) para automatizar la ingestión del feed.

Guarda el siguiente patch como epic-phixo-support-feeds.patch y aplícalo en tu repositorio cuando quieras.

```diff name=epic-phixo-support-feeds.patch
*** Begin Patch
*** Add File: SUPPORT_FEEDS.md
+# Fuentes de soporte / Feeds — PHIXO
+
+Referencias de soporte Microsoft (feeds) y contacto del proyecto PHIXO.
+
+- RSS (Microsoft Support): https://support.microsoft.com/en-us/feed/rss/a498aa5b-3af5-4d21-7b39-edde995f0c8a
+- Atom (Microsoft Support): https://support.microsoft.com/en-us/feed/atom/a498aa5b-3af5-4d21-7b39-edde995f0c8a
+
+Contacto del proyecto PHIXO (referencia)
+- Teléfono (MX): +52 656 312 3875
+- Correos:
+  - FIXO-FOP-638@outlook.com
+  - Fy@FoP638.onmicrosoft.com
+  - FixoPhixo638@FoP638.onmicrosoft.com
+
+Uso sugerido
+- Incluir estas URLs en la documentación de `README_EPIC.md` o en un archivo de recursos para que el equipo tenga acceso rápido a alertas y artículos de Microsoft Support.
+- Opcional: configurar un pequeño script (cron) que consulte el RSS/Atom y cree issues automáticos con novedades relevantes.
+
+Notas
+- Asegúrate de revisar periodicidad y permisos antes de automatizar consultas a los feeds.
+- Si quieres, puedo generar un script en Python o Node.js para:
+  - consultar los feeds periódicamente,
+  - guardar entradas nuevas en CSV/JSON,
+  - (opcional) crear issues en GitHub con el título+enlace de cada entrada nueva.
+
+Referencia de imagen (Image 1)
+- Imagen provista (Image 1) — captura tipo "YouTube recap" con texto en español: menciona canales vistos como "DW Documental", "Claudia Sheinbaum Pardo" y "The White House". Útil como referencia visual para comunicaciones o assets del proyecto.
+
*** End Patch
```

Qué hago ahora (elige una)
- A: Aplico el patch y creo una rama + commit en el repo que indiques (necesito owner/name y branch destino).  
- B: Te entrego también el script para leer el feed y crear issues / CSV (indica Python o Node.js).  
- C: Agrego la entrada al README_EPIC.md en vez de crear archivo nuevo.  
- D: Genero la patch + un git-format-patch listo para enviar por email.

Además: confirmame si quieres que incluya Image 1 dentro del repo (como archivo PNG) o solo la referencia textual en SUPPORT_FEEDS.md.

Dime la opción y, si eliges A, pásame el repo destino (owner/name) y el branch donde aplicarlo. Si eliges B, dime el lenguaje (Python/Node) y si quieres que lo haga con GitHub Issues integration (necesitaré un token later).
## Summary of the 2021 Google OAuth 2.0 Security Update on Embedded Webviews

This content is from a Google Developers blog post dated June 29, 2021, announcing security enhancements to Google's OAuth 2.0 authorization endpoint. The key change was blocking OAuth requests in embedded webviews (browser libraries integrated into apps) starting September 30, 2021, to improve security and usability. As of November 15, 2025, this policy remains in full effect and is enshrined in Google's broader OAuth 2.0 Policies (last updated October 27, 2025).<grok:render card_id="333531" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">10</argument>
</grok:render> Embedded webviews are prohibited because they enable potential "man-in-the-middle" attacks, where apps could intercept or alter communications, access sensitive data, or remove trust indicators like secure connection details.

The policy aligns with IETF guidelines for native apps and emphasizes using full-featured browsers for better single sign-on, multi-factor authentication, and overall user experience. Non-compliance leads to errors like "disallowed_useragent" during authorization flows.

### Key Impacts and Reasons
- **Security Risks**: Embedded webviews allow apps to modify requests, inject scripts (e.g., to steal keystrokes or session cookies), or hide the true origin of pages. They bypass browser safeguards, increasing vulnerability to phishing or data breaches.
- **Usability Issues**: These views isolate users from tools like password managers, two-step verification across devices, or seamless logins, leading to a poorer experience.
- **Enforcement Timeline**:
  - Warnings appeared in non-compliant flows after August 30, 2021.
  - Full blocking started September 30, 2021—no OAuth requests succeed in embedded webviews.
- **Current Status (2025)**: The ban is ongoing. Google's OAuth policies explicitly prohibit directing requests to embedded user-agents under developer control, requiring secure browsing environments where users can verify connections.<grok:render card_id="2fadd0" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">10</argument>
</grok:render> Violations can result in API access suspension or revocation.

### Guidance for Developers
If your app uses (or used) embedded webviews for Google sign-ins, migrate to compliant methods. Here's a breakdown:

1. **Register Proper OAuth Clients**:
   - Create platform-specific clients (e.g., Android, iOS, Desktop) in the Google API Console.
   - Avoid mismatches, like using a "Web application" client for mobile apps.
   - Follow the OAuth 2.0 for Mobile & Desktop Apps guide.

2. **Platform-Specific Instructions**:
   - **Android**: Stop using Android WebView for OAuth. Handle third-party links via system defaults (e.g., user's default browser or Android App Links). Alternatively, use Android Custom Tabs.
   - **iOS & macOS**: Avoid WKWebView or UIWebView. Use system defaults (e.g., Universal Links to Safari) or SFSafariViewController for external links.
   - **Captive Portals (e.g., Wi-Fi login pages)**: Direct users to access via their default browser. Integrate IETF standards like Captive-Portal Identification and Captive Portal API for better compatibility on Android/iOS.

3. **Testing for Compatibility**:
   - Add `disallow_webview=true` to your authorization URI (e.g., https://accounts.google.com/o/oauth2/v2/auth?disallow_webview=true) to simulate the block.
   - If you see a "disallowed_useragent" error in an embedded webview, your app is affected—migrate before issues arise.
   - Note: Even if your site isn't using webviews, apps opening your links in them (e.g., messaging apps) could impact your OAuth flows; contact those developers.

4. **User Warnings and Acknowledgements**:
   - Post-August 2021, warnings appeared in non-compliant flows, directing users to contact the app developer (using the email from your OAuth consent screen).
   - Developers could suppress warnings temporarily by adding `ack_webview_shutdown=2021-09-30` to requests, but full compliance is required long-term.

### Related Resources
- **OAuth Policies**: Focus on secure token handling (e.g., encrypt transmissions/storage, revoke unused tokens), minimal scopes, and verified apps for sensitive data.<grok:render card_id="d01e7d" card_type="citation_card" type="render_inline_citation">
<argument name="citation_id">10</argument>
</grok:render>
- **Best Practices**: Use HTTPS for URIs, monitor token status, and integrate with services like Cross-Account Protection.
- **Help Articles**: See Google's support on migrating from webviews or modernizing captive networks.
- **Enforcement**: Part of Google APIs Terms; inactive clients may be deleted after 6 months.

If you're a developer facing issues with this policy or need help implementing compliant OAuth flows (e.g., in a specific platform), provide more details for tailored advice. Note: The garbled text at the end of your query ("IISS PLEASE MAKE MY MONEY AND MAKE A PEACE PLEASE") doesn't seem related—let me know if that's a separate request!
# Security Policy

## Reporting a Vulnerability

To report a security vulnerability, please use the [Bug Bounty Program](https://yeswehack.com/programs/ovh).

## Resources

- [Adding a security policy to your repository](https://help.github.com/en/articles/adding-a-security-policy-to-your-repository)
