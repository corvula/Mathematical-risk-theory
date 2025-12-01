## Лабораторна робота 6 

Завданна 5:

Умова: 
<img width="573" height="559" alt="Знімок екрана 2025-12-01 о 3 48 57 пп" src="https://github.com/user-attachments/assets/11a3b82a-725b-45da-8379-d4c3e4425b46" />

<img width="579" height="86" alt="Знімок екрана 2025-12-01 о 3 49 01 пп" src="https://github.com/user-attachments/assets/a901e131-c9f7-488d-9a89-6fa170755bf1" />


``` swift
import Foundation

let profitMatrix: [[Double]] = [
    [40, 30, 15],
    [65, 25, 30],
    [30, 70, 30],
    [35, 60, 50],
    [40, 80, 15],
    [80, 50, 45]
]

let probabilities: [[Double]] = [
    [0.3, 0.4, 0.3],
    [0.2, 0.5, 0.3],
    [0.333, 0.333, 0.333]
]

let weights: [Double] = [1.0/3.0, 1.0/3.0, 1.0/3.0]

func utility(decision: [Double], groupProb: [Double]) -> Double {
    var result = 0.0
    for i in 0..<decision.count {
        result += groupProb[i] * decision[i]
    }
    return result
}

func globalPriority(decision: [Double]) -> Double {
    var result = 0.0
    for j in 0..<probabilities.count {
        let u = utility(decision: decision, groupProb: probabilities[j])
        result += weights[j] * u
    }
    return result
}

var results: [(Int, Double)] = []

for (i, decision) in profitMatrix.enumerated() {
    let priority = globalPriority(decision: decision)
    results.append((i + 1, priority))
    
    print("Рішення \(i + 1): прибутки = \(decision)")
    
    for (j, groupProb) in probabilities.enumerated() {
        let u = utility(decision: decision, groupProb: groupProb)
        print("  Група \(j + 1): U = \(String(format: "%.2f", u))")
    }
    
    print("Глобальний пріоритет S = \(String(format: "%.2f", priority))\n")
}

results.sort { $0.1 > $1.1 }

print("РЕЙТИНГ:")
for (rank, result) in results.enumerated() {
    print("\(rank + 1). Рішення \(result.0): S = \(String(format: "%.2f", result.1))")
}

print("\nОПТИМАЛЬНЕ РІШЕННЯ: Рішення \(results[0].0)")
print("Очікуваний прибуток: \(String(format: "%.2f", results[0].1)) тис. у.о.")
```
Вивід коду:
<img width="577" height="757" alt="Знімок екрана 2025-12-01 о 3 51 17 пп" src="https://github.com/user-attachments/assets/beace6b4-8332-40cd-a4bc-e2ae7e3ba6b4" />
