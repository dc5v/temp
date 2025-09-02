3.3v-5.7v 핫스왑회로(음극양극 상관없이)

* 여기에 커패시터를 추가하면 좀 더 안정적인 전압을 공급해 
MCU 코마상황(MCU가 DEEP SLEEP -> WAKEUP 트리거 시 깨어나지 못하고 혼수상태가 
지속되는-경우)에 아주 드라마틱한 도움을 준다 (경험담).

* 전압강하를 해결하려고 dd, tr 다해봤지만 mosfet을 이길 수 없으셈.

```mermaid

graph LR

    VCC[["I/O"]]
    GND[["I/O"]]
    ESP32S3P[[ESP32S3+]]
    ESP32S3N[[ESP32S3-]]

    subgraph M1[MOSFET 1]
      M1D(D)
      M1S(S)
      M1G(G)
    end

    subgraph M4[MOSFET 4]
      direction LR
      M4D(D)
      M4S(S)
      M4G(G)
    end

    subgraph M2[MOSFET 2]
      M2D(D)
      M2S(S)
      M2G(G)
    end

    subgraph M3[MOSFET 3]
      M3D(D)
      M3S(S)
      M3G(G)
    end
  
    VCC ===> IN1(("±"))
    IN1 ---> M1S
    IN1 ---> M2S

    GND ===> IN2(("±"))
    IN2 ---> M3S
    IN2 ---> M4S
    

    M3G -.-> DOTP((+)) 
    M3D -.-> DOTP
    M1D -.-> DOTP
    M1G -.-> DOTP
    DOTP ===> ESP32S3P

    
    M4G -.-> DOTN(("-"))
    M4D -.-> DOTN    
    M2G -.-> DOTN
    M2D -.-> DOTN
    DOTN ===> ESP32S3N
```
