# verificar-integridad

Verificador de integridad de archivos en Python (FIM casero). Vigila archivos importantes y avisa si alguno cambió, se borró o apareció uno nuevo, sin que vos lo tocaras. Sirve para detectar que un atacante o malware modificó archivos del sistema.

## Cómo funciona

Usa **hash** (SHA-256), igual que el buscador de duplicados, pero aplicado a seguridad:

1. **Línea base** (`init`): calcula el hash de cada archivo cuando confiás en ellos. Es la "foto" del estado correcto.
2. **Chequeo** (`check`): recalcula los hashes y compara con la foto.
   - Mismo hash → intacto.
   - Hash distinto → **modificado**.
   - Falta → **borrado**.
   - Aparece uno → **nuevo** (posible intruso).

El hash detecta cualquier cambio, hasta de un solo byte. Es imposible modificar un archivo sin que cambie su hash.

Esto se llama **FIM (File Integrity Monitoring)**, parte de los sistemas de detección de intrusos basados en host (HIDS). Herramientas profesionales como AIDE o Tripwire hacen lo mismo.

## Requisitos

Solo Python 3.

## Uso

```bash
python3 verificar_integridad.py init ~/carpeta     # crear línea base
python3 verificar_integridad.py check ~/carpeta    # chequear cambios
```

La línea base se guarda en `integridad_base.json`. Si vos hacés cambios legítimos, volvés a correr `init` para actualizarla.

## Para qué sirve en la práctica

- Vigilar archivos de configuración del sistema (`/etc`) y detectar cambios no autorizados.
- Confirmar que los binarios de un programa no fueron reemplazados por versiones infectadas.
- Monitorear una carpeta de un sitio web para detectar que subieron código malicioso.

## Roadmap

- [x] v1: línea base y detección de modificados/borrados/nuevos
- [ ] v2: guardar varias líneas base (una por carpeta vigilada)
- [ ] v3: correr con cron y avisar solo si hay cambios
- [ ] v4: reporte con fecha de cada chequeo
