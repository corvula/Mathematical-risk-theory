## Лабораторна номер 2, варіант 5

<img width="668" height="276" alt="Знімок екрана 2025-09-08 о 3 17 09 пп" src="https://github.com/user-attachments/assets/cb93ab96-2adc-42d9-a16e-5e783b49f950" />


```swift
import Foundation

struct Option {
    let name: String
    let outcomes: [(prob: Double, profit: Double)]
}

func analyzeOption(_ option: Option) -> (mean: Double, variance: Double, risk: Double) {
    var mean = 0.0
    for outcome in option.outcomes {
        mean += outcome.prob * outcome.profit
    }
    
    var variance = 0.0
    for outcome in option.outcomes {
        let diff = outcome.profit - mean
        variance += outcome.prob * diff * diff
    }
    
    let sigma = sqrt(variance)
    
    let risk = mean != 0 ? sigma / mean : Double.infinity
    return (mean, variance, risk)
}
let options = [
    Option(name: "1 корпорація", outcomes: [(0.6, 8.0), (0.4, -0.5)]),
    Option(name: "2 корпорації", outcomes: [(0.7, 12.0), (0.3, -0.5)]),
    Option(name: "Асоціація", outcomes: [(0.3, 25.0), (0.7, -1.0)])
]
for option in options {
    let result = analyzeOption(option)
    print("Варіант: \(option.name)")
    print("  Математичне сподівання = \(String(format: "%.2f", result.mean)) млн грн")
    print("  Варіація = \(String(format: "%.2f", result.variance))")
    print("  Коефіцієнт ризику = \(String(format: "%.2f", result.risk))\n")
}

var bestOption = options[0]
var bestMean = analyzeOption(bestOption).mean

for i in 1..<options.count {
    let current = options[i]
    let currentMean = analyzeOption(current).mean
    
    if currentMean > bestMean {
        bestOption = current
        bestMean = currentMean
    }
}
print("Найвигідніший варіант: \(bestOption.name)")

```
<img width="444" height="326" alt="Знімок екрана 2025-09-22 о 3 12 55 пп" src="https://github.com/user-attachments/assets/0f1e969e-f62a-4783-8e5a-bb149e5c9b5e" />


