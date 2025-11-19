## Лабораторна робота 5

Умова:
![2025-11-19 13 54 38](https://github.com/user-attachments/assets/10b9d142-81aa-495e-a370-3ec30c70b945)

Для коректного виводу програми я замінила знаки - <img width="64" height="41" alt="Знімок екрана 2025-11-19 о 2 11 51 пп" src="https://github.com/user-attachments/assets/ee4cd034-5150-4c2f-a32d-1352d0c69f59" /> 
на - Y (n), де n = 1,2,3 

``` swift
import Foundation

struct DecisionProblem {
    let decisions: [String]
    let demandStates: [String]
    let payoffMatrix: [[Double]]
    let probabilities: [Double]?

    func bayesCriterion(probabilities: [Double]) -> (decision: String, expectedValue: Double) {
        var maxExpectedValue = -Double.infinity
        var bestDecision = ""
        print("\nКритерій Байєса ")
        print("Математичне очікування для кожного рішення:")
        
        for (i, decision) in decisions.enumerated() {
            var expectedValue = 0.0
            var calculation = "\(decision): "
            
            for (j, prob) in probabilities.enumerated() {
                expectedValue += payoffMatrix[i][j] * prob
                calculation += "\(payoffMatrix[i][j]) × \(prob)"
                if j < probabilities.count - 1 {
                    calculation += " + "
                }
            }
            calculation += " = \(String(format: "%.2f", expectedValue))"
            print(calculation)
            
            if expectedValue > maxExpectedValue {
                maxExpectedValue = expectedValue
                bestDecision = decision
            }
        }
        return (bestDecision, maxExpectedValue)
    }

    func laplaceCriterion() -> (decision: String, averageValue: Double) {
        let uniformProb = 1.0 / Double(demandStates.count)
        let probs = Array(repeating: uniformProb, count: demandStates.count)
        
        print("\nКритерій Лапласа (рівноймовірні стани)")
        print("Ймовірність кожного стану: \(String(format: "%.3f", uniformProb))")
        
        let result = bayesCriterion(probabilities: probs)
        return (decision: result.decision, averageValue: result.expectedValue)
    }

    func waldCriterion() -> (decision: String, minValue: Double) {
        var maxOfMins = -Double.infinity
        var bestDecision = ""
        
        print("\nКритерій Вальда")
        print("Мінімальні прибутки для кожного рішення:")
        
        for (i, decision) in decisions.enumerated() {
            let minValue = payoffMatrix[i].min() ?? -Double.infinity
            print("\(decision): min = \(minValue)")
            
            if minValue > maxOfMins {
                maxOfMins = minValue
                bestDecision = decision
            }
        }
        
        return (bestDecision, maxOfMins)
    }
    
    func maximaxCriterion() -> (decision: String, maxValue: Double) {
        var maxOfMaxs = -Double.infinity
        var bestDecision = ""
        
        print("\nКритерій максимакса (оптимістичний)")
        print("Максимальні прибутки для кожного рішення:")
        
        for (i, decision) in decisions.enumerated() {
            let maxValue = payoffMatrix[i].max() ?? -Double.infinity
            print("\(decision): max = \(maxValue)")
            
            if maxValue > maxOfMaxs {
                maxOfMaxs = maxValue
                bestDecision = decision
            }
        }
        return (bestDecision, maxOfMaxs)
    }

    func hurwiczCriterion(alpha: Double = 0.5) -> (decision: String, value: Double) {
        var maxHurwicz = -Double.infinity
        var bestDecision = ""
        
        print("\nКритерій Гурвіца ")
        print("Коефіцієнт оптимізму a = \(alpha)")
        print("H = a × max + (1 - a) × min")
        
        for (i, decision) in decisions.enumerated() {
            let minValue = payoffMatrix[i].min() ?? 0
            let maxValue = payoffMatrix[i].max() ?? 0
            let hurwiczValue = alpha * maxValue + (1 - alpha) * minValue
            
            print("\(decision): \(alpha) × \(maxValue) + \(1-alpha) × \(minValue) = \(String(format: "%.2f", hurwiczValue))")
            
            if hurwiczValue > maxHurwicz {
                maxHurwicz = hurwiczValue
                bestDecision = decision
            }
        }
        return (bestDecision, maxHurwicz)
    }

    func savageCriterion() -> (decision: String, minRegret: Double) {
        print("\nКритерій Севіджа (мінімакс жалю)")

        var regretMatrix: [[Double]] = []
        
        print("Матриця жалю:")
        print("Стани попиту: \(demandStates.joined(separator: "\t"))")

        for j in 0..<demandStates.count {
            var maxInColumn = -Double.infinity
            for i in 0..<decisions.count {
                maxInColumn = max(maxInColumn, payoffMatrix[i][j])
            }

            for i in 0..<decisions.count {
                if regretMatrix.count <= i {
                    regretMatrix.append([])
                }
                let regret = maxInColumn - payoffMatrix[i][j]
                regretMatrix[i].append(regret)
            }
        }

        for (i, decision) in decisions.enumerated() {
            let regrets = regretMatrix[i].map { String(format: "%.1f", $0) }.joined(separator: "\t")
            print("\(decision):\t\(regrets)")
        }

        var minMaxRegret = Double.infinity
        var bestDecision = ""
        
        print("\nМаксимальні жалі:")
        for (i, decision) in decisions.enumerated() {
            let maxRegret = regretMatrix[i].max() ?? Double.infinity
            print("\(decision): max regret = \(maxRegret)")
            
            if maxRegret < minMaxRegret {
                minMaxRegret = maxRegret
                bestDecision = decision
            }
        }
        return (bestDecision, minMaxRegret)
    }

    func printPayoffMatrix() {
        print("МАТРИЦЯ ПРИБУТКІВ (тис. грн)")
        print("\nСтани попиту: \(demandStates.joined(separator: "\t"))")
        print(String(repeating: "─", count: 50))
        
        for (i, decision) in decisions.enumerated() {
            let values = payoffMatrix[i].map { String(format: "%.1f", $0) }.joined(separator: "\t")
            print("\(decision):\t\(values)")
        }
        print()
    }
}

let problem = DecisionProblem(
    decisions: ["x1", "x2", "x3"],
    demandStates: ["Y(1)", "Y(2)", "Y(3)"],
    payoffMatrix: [
        [6.0, 3.5, 0.5],
        [6.5, 7.0, 4.0],
        [3.5, 3.5, 8.5]
    ],
    probabilities: nil
)

print("ЗАДАЧА ПРИЙНЯТТЯ РІШЕНЬ ДЛЯ АЗС")

print("\nВласник АЗС вирішує, як розподілити кошти між:")
print("x1 - продаж засобів догляду за автомобілем")
print("x2 - продаж дрібних запчастин")
print("x3 - продаж газетно-журнальної продукції")

problem.printPayoffMatrix()

let waldResult = problem.waldCriterion()
print("\nОптимальне рішення: \(waldResult.decision) з гарантованим прибутком \(waldResult.minValue) тис. грн")

let maximaxResult = problem.maximaxCriterion()
print("\nОптимальне рішення: \(maximaxResult.decision) з максимальним прибутком \(maximaxResult.maxValue) тис. грн")

let hurwiczResult = problem.hurwiczCriterion(alpha: 0.6)
print("\nОптимальне рішення: \(hurwiczResult.decision) зі значенням \(String(format: "%.2f", hurwiczResult.value)) тис. грн")

let savageResult = problem.savageCriterion()
print("\nОптимальне рішення: \(savageResult.decision) з мінімальним жалем \(savageResult.minRegret) тис. грн")

let laplaceResult = problem.laplaceCriterion()
print("\nОптимальне рішення: \(laplaceResult.decision) з очікуваним прибутком \(String(format: "%.2f", laplaceResult.averageValue)) тис. грн")

print("АНАЛІЗ З ВІДОМИМИ ЙМОВІРНОСТЯМИ")

let probabilities = [0.3, 0.5, 0.2]
let bayesResult = problem.bayesCriterion(probabilities: probabilities)
print("\nЙмовірності станів: Y(1) =\(probabilities[0]), Y(2) =\(probabilities[1]), Y(3) =\(probabilities[2])")
print("\nОптимальне рішення: \(bayesResult.decision) з очікуваним прибутком \(String(format: "%.2f", bayesResult.expectedValue)) тис. грн")


print("\nРЕКОМЕНДАЦІЇ")
print("""

Вибір стратегії залежить від ставлення власника до ризику:

• Песиміст (Вальд): \(waldResult.decision) - гарантує мінімальний прибуток
• Оптиміст (максимакс): \(maximaxResult.decision) - максимальний потенціал
• Реаліст (Гурвіц): \(hurwiczResult.decision) - компроміс
• Обережний (Севідж): \(savageResult.decision) - мінімізує можливі втрати
• Раціональний (Лаплас/Байєс): \(laplaceResult.decision) - найкращий середній результат
""")
```
### Результати:
ЗАДАЧА ПРИЙНЯТТЯ РІШЕНЬ ДЛЯ АЗС

Власник АЗС вирішує, як розподілити кошти між:
x1 - продаж засобів догляду за автомобілем
x2 - продаж дрібних запчастин
x3 - продаж газетно-журнальної продукції
МАТРИЦЯ ПРИБУТКІВ (тис. грн)

Стани попиту: Y(1)	Y(2)	Y(3)
──────────────────────────────────────────────────
x1:	6.0	3.5	0.5
x2:	6.5	7.0	4.0
x3:	3.5	3.5	8.5


Критерій Вальда
Мінімальні прибутки для кожного рішення:
x1: min = 0.5
x2: min = 4.0
x3: min = 3.5

Оптимальне рішення: x2 з гарантованим прибутком 4.0 тис. грн

Критерій максимакса (оптимістичний)
Максимальні прибутки для кожного рішення:
x1: max = 6.0
x2: max = 7.0
x3: max = 8.5

Оптимальне рішення: x3 з максимальним прибутком 8.5 тис. грн

Критерій Гурвіца 
Коефіцієнт оптимізму a = 0.6
H = a × max + (1 - a) × min
x1: 0.6 × 6.0 + 0.4 × 0.5 = 3.80
x2: 0.6 × 7.0 + 0.4 × 4.0 = 5.80
x3: 0.6 × 8.5 + 0.4 × 3.5 = 6.50

Оптимальне рішення: x3 зі значенням 6.50 тис. грн

Критерій Севіджа (мінімакс жалю)
Матриця жалю:
Стани попиту: Y(1)	Y(2)	Y(3)
x1:	0.5	3.5	8.0
x2:	0.0	0.0	4.5
x3:	3.0	3.5	0.0

Максимальні жалі:
x1: max regret = 8.0
x2: max regret = 4.5
x3: max regret = 3.5

Оптимальне рішення: x3 з мінімальним жалем 3.5 тис. грн

Критерій Лапласа (рівноймовірні стани)
Ймовірність кожного стану: 0.333

Критерій Байєса 
Математичне очікування для кожного рішення:
x1: 6.0 × 0.3333333333333333 + 3.5 × 0.3333333333333333 + 0.5 × 0.3333333333333333 = 3.33
x2: 6.5 × 0.3333333333333333 + 7.0 × 0.3333333333333333 + 4.0 × 0.3333333333333333 = 5.83
x3: 3.5 × 0.3333333333333333 + 3.5 × 0.3333333333333333 + 8.5 × 0.3333333333333333 = 5.17

Оптимальне рішення: x2 з очікуваним прибутком 5.83 тис. грн
АНАЛІЗ З ВІДОМИМИ ЙМОВІРНОСТЯМИ

Критерій Байєса 
Математичне очікування для кожного рішення:
x1: 6.0 × 0.3 + 3.5 × 0.5 + 0.5 × 0.2 = 3.65
x2: 6.5 × 0.3 + 7.0 × 0.5 + 4.0 × 0.2 = 6.25
x3: 3.5 × 0.3 + 3.5 × 0.5 + 8.5 × 0.2 = 4.50

Ймовірності станів: Y(1) =0.3, Y(2) =0.5, Y(3) =0.2

Оптимальне рішення: x2 з очікуваним прибутком 6.25 тис. грн

РЕКОМЕНДАЦІЇ

Вибір стратегії залежить від ставлення власника до ризику:

• Песиміст (Вальд): x2 - гарантує мінімальний прибуток
• Оптиміст (максимакс): x3 - максимальний потенціал
• Реаліст (Гурвіц): x3 - компроміс
• Обережний (Севідж): x3 - мінімізує можливі втрати
• Раціональний (Лаплас/Байєс): x2 - найкращий середній результат
Program ended with exit code: 0
