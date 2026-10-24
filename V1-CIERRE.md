# Cierre V1 — TCL G09 / CleanYourTV

Fecha: 23 de octubre de 2026. V1 cerrada por decisión del propietario.
Este documento es el punto de continuación local. Recoge la evidencia existente y las validaciones manuales comunicadas por el propietario; el cierre no ejecutó ADB, pruebas, compilaciones ni cambios en el TV.

## ESTADO V1

- CleanYourTV (`com.gathin.tvhome/.MainActivity`) es HOME efectivo en el TCL 65P7K, plataforma G09, Android 12.
- `com.google.android.apps.tv.launcherx` y `com.google.android.tungsten.setupwraith` están deshabilitados únicamente para usuario 0. No se debe reactivarlos al retomar sin nueva autorización.
- El launcher funciona y las aplicaciones, incluidas Netflix, Prime Video y YouTube, se abren desde sus tiles.
- HOME, back, navegación, volumen, mute, micrófono y los demás controles ya validados permanecen funcionales. El botón de tres líneas abre ajustes rápidos TCL; se conserva ese comportamiento útil, sin afirmar equivalencia completa con el mando stock.
- Ethernet funciona. El propietario validó que Wi-Fi conecta correctamente al desconectar Ethernet; con Ethernet conectado queda guardado y Ethernet tiene prioridad. No atribuir este comportamiento a setupwraith. El diagnóstico de Wi-Fi queda cerrado.
- No se habilitó ningún helper/AccessibilityService ni se concedieron permisos adicionales. No se hizo debloat adicional ni reboot de validación; la persistencia después de reiniciar no se considera probada.

## LIMITACIÓN CONOCIDA

Los botones físicos dedicados Netflix, Prime Video y YouTube generan eventos pero no abren sus aplicaciones. La evidencia registra que TCL intercepta esas teclas y dirige el manejo hacia `com.tcl.ttvs`. No se encontró una ruta segura para reasignarlas con los privilegios actuales; esto no demuestra la inexistencia de una API privada aún desconocida.

Accessibility/InputKeyService podría interceptarlas, pero requiere filtrado global y existe riesgo conocido de regresión en repetición de volumen en G09. No se habilitó ese mecanismo. Los keylayouts/global_keys/overlays relevantes requieren privilegios invasivos o modificación del firmware para alterarlos; esa vía se descarta para V1. Las tres aplicaciones continúan funcionando normalmente desde los tiles de CleanYourTV.

## DECISIÓN

Esta limitación NO bloquea V1. Se investigará exhaustivamente en una fase posterior cuando haya nuevamente presupuesto de Codex. Se conserva el estado actual, sin nuevas pruebas ni cambios funcionales.

## PENDIENTE V2 / CONTINUACIÓN

- investigar exhaustivamente una solución segura para Netflix/Prime/YouTube dedicados;
- estudiar con mayor profundidad `com.tcl.ttvs` y su manejo de broadcasts;
- estudiar si existe una API/configuración TCL privada reutilizable sin root;
- evaluar experimentalmente InputKeyService en un entorno controlado únicamente si existe rollback completo, prestando especial atención a autorepeat de volumen;
- revisar cualquier diferencia restante entre el comportamiento del mando stock y CleanYourTV;
- NO asumir que debemos restaurar LauncherX.

## Código y APK de referencia

Base Git: `f9f0c5353e65c5003d5f2f45784a30c5dee19ecb` más el cambio local de [config.xml](launcher/app/src/main/res/values/config.xml):

- `live_tv_package`: `com.mediatek.wwtv.tvcenter` → `com.tcl.tv`.
- `key_filter`: `true` → `false`.

Son ajustes definitivos para este TV, no una propuesta de cambiar los defaults de todos los televisores upstream. No hay cambios experimentales versionados de Java/helpers en el diff de cierre.

APK V1: [app-release.apk](launcher/app/build/outputs/apk/release/app-release.apk), paquete `com.gathin.tvhome`, versión 2.7 / código 8, 486947 bytes.

SHA-256 recalculado localmente al cerrar:

```text
2df42688e65b9aaf22b835ee4c26fe262b85b9f9bde05216f903e14bb16ac047
```

Ruta absoluta local:

```text
/Users/gabrielintriago/GINTRICH/Repositorios/cleanyourtv/launcher/app/build/outputs/apk/release/app-release.apk
```

Los metadatos de compilación, firma y transferencia están archivados en `build-summary.json`, `apk-signature.txt`, `apk-badging.txt`, `APK-SHA256.txt`, `TV-APK-SHA256.txt` y `verified-install-log.json` dentro del backup de FASE 2. No se recompiló ni verificó nuevamente el TV durante este cierre.

## Backups e índice de evidencia

Todas las rutas siguientes son locales, privadas y excluidas de Git. Los informes antiguos describen momentos distintos: no deben interpretarse como el estado actual ni usarse para repetir automáticamente procedimientos.

| Evidencia | Ruta |
|---|---|
| Inventario original, propiedades, paquetes y settings | [FASE 1](backup/tcl-65p7k-fase1-20261023-194708-0500/INFORME.md) |
| Cronología, configuración original/compilada, instalación y hashes | [FASE 2](backup/tcl-65p7k-fase2-20261023-200638-0500/ESTADO.md) |
| Diagnóstico de apertura inicial | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/diagnostico-apertura-20261023-202405/INFORME.md) |
| Intento HOME anterior, fallido y revertido | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/cambio-home-20261023-203002/INFORME.md) |
| Resolución HOME y prioridades | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/diagnostico-resolucion-home-20261023-203240/INFORME.md) |
| Evaluación por componentes, sin aplicar | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/evaluacion-componentes-20261023-204221/INFORME.md) |
| Precheck G09 bloqueado, sin cambios | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/g09-home-minimo-20261023-204348/INFORME.md) |
| Aplicación G09 exitosa: estado original, comandos y verificación | [Informe autoritativo de HOME](backup/tcl-65p7k-fase2-20261023-200638-0500/g09-home-autorizado-20261023-205517/INFORME.md) |
| Captura de CleanYourTV como HOME | [Screenshot](backup/tcl-65p7k-fase2-20261023-200638-0500/g09-home-autorizado-20261023-205517/clean-your-tv-home.png) |
| Regresiones, timestamps, eventos y diferencias de paquetes | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/diagnostico-regresiones-20261023-211413/INFORME.md) |
| Revisión del helper, sin implementación | [Informe](backup/tcl-65p7k-fase2-20261023-200638-0500/revision-solucion-mando-20261023-213751/INFORME.md) |
| Última investigación local sin accesibilidad | [Conclusión](backup/tcl-65p7k-fase2-20261023-200638-0500/cierre-botones-sin-accesibilidad-20261023-214111/CONCLUSION.md) |

El directorio de regresiones contiene `keycodes-confirmados.json`, `package-state-diff.json`, `after-logcat.txt` y evidencias por síntoma. Las hipótesis Wi-Fi de informes previos quedan superadas por la validación manual final indicada en ESTADO V1. Los archivos vacíos/de error de captura no acreditan ausencia de eventos.

## Rollback conservado, no ejecutado

El rollback completo del cambio HOME está en [ROLLBACK-ADB-COMMANDS.txt](backup/tcl-65p7k-fase2-20261023-200638-0500/g09-home-autorizado-20261023-205517/ROLLBACK-ADB-COMMANDS.txt) y [ROLLBACK-DESDE-MAC.txt](backup/tcl-65p7k-fase2-20261023-200638-0500/g09-home-autorizado-20261023-205517/ROLLBACK-DESDE-MAC.txt), junto con `ESTADOS-ORIGINALES.json` y los registros de recuperación de ambos paquetes.

Es un procedimiento de emergencia que revierte V1: devuelve los paquetes a su estado original y restaura LauncherX como HOME, sin depender de desinstalar CleanYourTV. No recupera credenciales Wi-Fi eliminadas. No ejecutarlo al retomar ni interpretar su existencia como autorización; el propietario quiere conservar CleanYourTV y ambos paquetes deshabilitados.

## Clasificación local y recomendación de commit

- **Definitivo V1:** las dos adaptaciones de `launcher/app/src/main/res/values/config.xml` y este documento. Recomendados para un commit local de cierre; no se hizo commit.
- **Diagnóstico/artefactos privados:** `backup/`, `shots/`, `work/`, APK/build, toolchain local, `launcher/local.properties`, `tools/venv/` y `tools/adbkey/`. No incluir en el commit: pueden contener identificadores, credenciales/claves, binarios o datos personales.
- **Histórico/experimental, no implementación V1:** scripts de intentos de transferencia, pruebas HOME revertidas, evaluaciones por componentes, prechecks bloqueados y capturas fallidas; fuentes AOSP descargadas y wrappers de operación en `work/`. Se conservan sin revertir ni eliminar. Algunos scripts modifican el TV: no ejecutarlos automáticamente.

Para continuar, leer primero este cierre y después solo la evidencia necesaria para V2. No repetir el diagnóstico completo, no asumir que los primeros informes describen el estado final y no aplicar soluciones sin nueva autorización.
