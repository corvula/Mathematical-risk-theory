## Лабораторна номер 2, варіант 5

<img width="668" height="276" alt="Знімок екрана 2025-09-08 о 3 17 09 пп" src="https://github.com/user-attachments/assets/cb93ab96-2adc-42d9-a16e-5e783b49f950" />

Для обчислення задачі використовували, ось цю формулу - 
<img width="581" height="205" alt="Знімок екрана 2025-09-08 о 3 17 34 пп" src="https://github.com/user-attachments/assets/e512d5f2-27cb-4af1-b4d6-840ce7ac99bc" />

```swift
struct Option {
    let name: String
    let success_prob: Double
    let success_profit: Double
    let fail_prob: Double
    let fail_profit: Double
    
    var efficiency: Double {
        (success_prob * success_profit) + (fail_prob * fail_profit)
    }
}

let options = [
    Option(name: "Приєднання до 1-ої компанії", success_prob: 0.6, success_profit: 8, fail_prob: 0.4, fail_profit: -0.5),
    Option(name: "Приєднання до 2-ої компанії", success_prob: 0.7, success_profit: 12, fail_prob: 0.3, fail_profit: -0.5),
    Option(name: "Створення асоціації", success_prob: 0.3, success_profit: 25, fail_prob: 0.7, fail_profit: -1)
]

var bestOptionName = ""
var maxEfficiency = 0.0

for option in options {
    print("Прибуток для варіанта '\(option.name)': \(option.efficiency) млн грн.")
    
    if option.efficiency > maxEfficiency {
        maxEfficiency = option.efficiency
        bestOptionName = option.name
    }
}

print("Найкращий варіант для вибору: '\(bestOptionName)' з очікуваним прибутком \(maxEfficiency) млн грн.")
```
<img width="566" height="114" alt="Знімок екрана 2025-09-08 о 3 18 47 пп" src="https://github.com/user-attachments/assets/0dd4140e-67a3-4261-a8a0-1b648f5ebf44" />
