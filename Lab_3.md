#  Лабораторна робота номер 3 

<img width="566" height="216" alt="Знімок екрана 2025-09-29 о 12 20 47 пп" src="https://github.com/user-attachments/assets/6e1869b7-7c82-46d2-a2e9-d6ff838ec745" />



```swift
import Foundation

let p1 = 0.4, p2 = 0.6

let v1 = 100_000 + 0.5*100_000
let v2 : Double = 100_000
let ev = p1*v1 + p2*v2


func U(_ x: Double) -> Double {
    return 2.0 * sqrt(x)
}

let eu = p1*U(v1) + p2*U(v2)
let ce = pow(eu / 2.0, 2.0)
let rp = ev - ce

print("EV (очікувані гроші) = \(ev)")
print("EU (очікувана корисність) = \(String(format: "%.4f", eu))")
print("CE (детермінований еквівалент) = \(String(format: "%.2f", ce))")
print("Risk Premium (премія за ризик) = \(String(format: "%.2f", rp))")
```


<img width="377" height="116" alt="Знімок екрана 2025-09-29 о 12 21 06 пп" src="https://github.com/user-attachments/assets/bb3849d4-4942-45b7-80c0-a4876a56fd5c" />

##  Економічна суть премія за ризиків 


<img width="586" height="211" alt="Знімок екрана 2025-09-29 о 12 21 51 пп" src="https://github.com/user-attachments/assets/ce2613b4-d8ac-49dc-8e46-151a75fe250f" />

