# VPN-hub-and-spoke-punto-a-multipunto-DMVPN-Fase-2-con-IKEv1-con-enrutamiento-din-mico


**Estudiante:** Juan Francisco Burgos Hiciano  

**Matrícula:** 2023-1981  

**Asignatura:** Seguridad en Redes  

📹 Video: https://youtu.be/nHUdejj8OB8

## Descripción

Implementación de una VPN DMVPN (Dynamic Multipoint VPN) Hub-and-Spoke utilizando:

- DMVPN Fase 2
- mGRE (Multipoint GRE)
- NHRP
- IPsec
- IKEv1 (ISAKMP)
- EIGRP

La topología está compuesta por un Hub (IOU3), dos Spokes (IOU2 e IOU4) y un router ISP que únicamente proporciona conectividad IP entre los sitios.

## Topología

- IOU3 → Hub DMVPN
- IOU2 → Spoke 1
- IOU4 → Spoke 2
- ISP → Router de tránsito
- PC1 → LAN Site A
- PC2 → LAN Hub
- PC3 → LAN Site B

![Topología VPN]https://github.com/jburgoshiciano-source/VPN-hub-and-spoke-punto-a-multipunto-DMVPN-Fase-2-con-IKEv1-con-enrutamiento-din-mico/blob/main/DMVPN.png

## Direccionamiento

| Dispositivo | Dirección |
|------------|-----------|
| Hub Tunnel0 | 172.16.0.1 |
| Spoke1 Tunnel0 | 172.16.0.2 |
| Spoke2 Tunnel0 | 172.16.0.3 |
| LAN Spoke1 | 192.168.1.0/24 |
| LAN Hub | 192.168.100.0/24 |
| LAN Spoke2 | 192.168.2.0/24 |

## Características de DMVPN Fase 2

- Utiliza NHRP para resolución dinámica.
- El Hub tiene deshabilitado:
  - split-horizon EIGRP
  - next-hop-self EIGRP
- Permite la creación automática de túneles spoke-to-spoke.
- El primer tráfico pasa por el Hub.
- El tráfico posterior utiliza túneles directos entre Spokes.

## Seguridad

### Fase 1 (IKEv1)

- AES-256
- SHA-256
- Diffie-Hellman Grupo 14
- Pre-Shared Key

### Fase 2 (IPsec)

- ESP-AES-256
- ESP-SHA256-HMAC
- Modo Transport

## Verificación

```bash
show dmvpn
show ip nhrp
show crypto isakmp sa
show crypto ipsec sa
show ip eigrp neighbors
show ip route eigrp
```

## Resultado esperado

Al generar tráfico entre PC1 y PC3:

1. El primer paquete atraviesa el Hub.
2. NHRP aprende la ubicación del destino.
3. Se forma un túnel dinámico spoke-to-spoke.
4. El tráfico posterior utiliza la ruta directa.

## Conclusión

DMVPN Fase 2 permite conectividad dinámica entre sitios remotos mediante NHRP y mGRE. La preservación del next-hop en EIGRP posibilita la formación automática de túneles spoke-to-spoke, reduciendo la dependencia del Hub para el tráfico de datos.
