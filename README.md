 # Dune-pds — Entorno DUNE/LArSoft

Contenedor Apptainer con LArSoft `v10_23_00` + `dunesw v10_24_00d00` instalados y configurados, listo para simular la
respuesta del Photon Detection System (PDS) de ProtoDUNE-HD.

## Requisitos

Tener [Apptainer](https://apptainer.org/docs/user/main/quick_start.html)
instalado:
```bash
sudo apt install apptainer
```

## Descargar el contenedor (~27 GB, una sola vez)

```bash
apptainer pull dune_pds.sif oras://ghcr.io/alistarco/dune-pds:v1
```

## Uso

```bash
apptainer shell dune_pds.sif
```

El entorno de LArSoft y dunesw se activa automaticamente al entrar.

Trabaja siempre desde una carpeta fuera del contenedor (por ejemplo
tu `$HOME`), ya que el `.sif` es de solo lectura:

```bash
mkdir -p ~/trabajo_pds && cd ~/trabajo_pds
```

## Cadena de simulacion del PDS (gen -> g4 -> detsim -> reco)

```bash
lar -c /larsoft_install/cadena_pds_protodunehd/01_gen.fcl    -n 10 -o pi_gen.root
lar -c /larsoft_install/cadena_pds_protodunehd/02_g4.fcl      pi_gen.root    -o pi_g4.root -n 10
lar -c /larsoft_install/cadena_pds_protodunehd/03_detsim.fcl  pi_g4.root     -o pi_detsim.root -n 10
lar -c /larsoft_install/cadena_pds_protodunehd/04_reco.fcl    pi_detsim.root -o pi_reco.root -n 10
```

Para consultar que se modificó en cada `.fcl` respecto a los
archivos oficiales de la colaboracion (y por que), ver:

/larsoft_install/cadena_pds_protodunehd/README.md


dentro del propio contenedor.

## Contenido relevante dentro del contenedor

- `/larsoft_install/cadena_pds_protodunehd/` — los 4 `.fcl` de la
  cadena + su README explicativo
- `/larsoft_install/dunesw/`, `/larsoft_install/dunecore/`, etc. —
  instalacion completa de dunesw v10_24_00d00

