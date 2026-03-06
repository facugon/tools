# **Referencia Técnica: Conector ATX 24 Pines**

**Vista del Conector:** Lado del cable (o vista superior del conector hembra de la fuente). **Clip de seguridad:** Mirando hacia arriba.

| **Pin** | **Color Típico** | **Señal** | **Descripción** |

| **1** | Naranja | \+3.3V | Alimentación chipset/RAM (Bajo voltaje) |

| **2** | Naranja | \+3.3V | Alimentación chipset/RAM |

| **3** | Negro | COM | Tierra (Ground) |

| **4** | Rojo | \+5V | Alimentación lógica TTL / USB / Discos |

| **5** | Negro | COM | Tierra (Ground) |

| **6** | Rojo | \+5V | Alimentación lógica |

| **7** | Negro | COM | Tierra (Ground) |

| **8** | Gris | PWR\_OK | **Power Good**: Indica a la PC que los voltajes son estables. |

| **9** | Púrpura | \+5VSB | **Standby**: Siempre tiene 5V si la fuente está enchufada (¡Cuidado\!) |

| **10** | Amarillo | \+12V | **CPU / Motores / PCIe**: La línea principal de potencia. |

| **11** | Amarillo | \+12V | Línea principal de potencia. |

| **12** | Naranja | \+3.3V | Alimentación chipset. |

*(Salto de fila \- Lado opuesto del clip)*

| **Pin** | **Color Típico** | **Señal** | **Descripción** |

| **13** | Naranja | \+3.3V | Alimentación chipset. |

| **14** | Azul | \-12V | Voltaje negativo (Raro hoy, usado en puertos serie viejos/audio). |

| **15** | Negro | COM | Tierra (Ground) |

| **16** | **VERDE** | **PS\_ON\#** | **ENCENDIDO**: Conectar a TIERRA (Negro) para prender la fuente. |

| **17** | Negro | COM | Tierra (Ground) |

| **18** | Negro | COM | Tierra (Ground) |

| **19** | Negro | COM | Tierra (Ground) |

| **20** | Blanco/NC | \-5V | Obsoleto/Vacío (No Conectado en fuentes modernas). |

| **21** | Rojo | \+5V | Alimentación lógica. |

| **22** | Rojo | \+5V | Alimentación lógica. |

| **23** | Rojo | \+5V | Alimentación lógica. |

| **24** | Negro | COM | Tierra (Ground) |

## **Instrucciones para el "Puenteo" (Arrancar sin Motherboard)**

Para usar una fuente vieja solo para alimentar periféricos (discos, ventiladores, o el Wolfdale modificado):

1. Ubica el **Pin 16 (Cable Verde)**.  
2. Ubica cualquier **Pin Negro (ej. Pin 15 o 17\)** que esté al lado.  
3. Usa un clip metálico o un cable pequeño para unirlos permanentemente.  
4. La fuente encenderá (el ventilador empezará a girar) y tendrás voltajes en todos los cables.

**Advertencia de Seguridad:**

* **Carga Mínima:** Algunas fuentes viejas necesitan tener algo conectado (como un disco duro viejo o un ventilador) para entregar voltajes estables. Si mides con multímetro y da valores raros, conéctale una carga.  
* **Cable Púrpura (+5VSB):** Este cable tiene corriente SIEMPRE que la fuente está enchufada a la pared, incluso si no hiciste el puente. No lo toques descuidadamente.