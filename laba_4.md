## Laba 4 
<img width="433" height="98" alt="Знімок екрана 2025-10-26 о 3 37 01 пп" src="https://github.com/user-attachments/assets/47a3afc8-c04a-4ccb-9dad-e18d5e3791b1" />

```
import Foundation

let E: [Double] = [0.6, 0.5, 0.4, 0.7]

let sigma: [Double] = [0.4, 0.3, 0.25, 0.5]

let rho: [[Double]] = [
    [1, 0.2, -0.3, 0.7],
    [0.2, 1, -0.5, 0.6],
    [-0.3, -0.5, 1, -0.3],
    [0.7, 0.6, -0.3, 1]
]

func multiplyMatrixVector(matrix: [[Double]], vector: [Double]) -> [Double] {
    var result = [Double](repeating: 0.0, count: matrix.count)
    for i in 0..<matrix.count {
        for j in 0..<vector.count {
            result[i] += matrix[i][j] * vector[j]
        }
    }
    return result
}

func dot(_ a: [Double], _ b: [Double]) -> Double {
    zip(a, b).map(*).reduce(0, +)
}

func invertMatrix(_ A: [[Double]]) -> [[Double]] {
    let n = A.count
    var a = A
    var inv = (0..<n).map { i in (0..<n).map { j in i == j ? 1.0 : 0.0 } }

    for i in 0..<n {
        var diag = a[i][i]
        if abs(diag) < 1e-10 { diag = 1e-10 }
        for j in 0..<n {
            a[i][j] /= diag
            inv[i][j] /= diag
        }
        for k in 0..<n {
            if k != i {
                let factor = a[k][i]
                for j in 0..<n {
                    a[k][j] -= factor * a[i][j]
                    inv[k][j] -= factor * inv[i][j]
                }
            }
        }
    }
    return inv
}

var C = Array(repeating: Array(repeating: 0.0, count: 4), count: 4)
for i in 0..<4 {
    for j in 0..<4 {
        C[i][j] = rho[i][j] * sigma[i] * sigma[j]
    }
}

let C_inv = invertMatrix(C)

let ones = [1.0, 1.0, 1.0, 1.0]

let numerator = multiplyMatrixVector(matrix: C_inv, vector: ones)
let denominator = dot(ones, multiplyMatrixVector(matrix: C_inv, vector: ones))

let w = denominator != 0 ? numerator.map { $0 / denominator } : [0.25, 0.25, 0.25, 0.25]

let E_portfolio = dot(w, E)

let temp = multiplyMatrixVector(matrix: C, vector: w)
let dotValue = dot(w, temp)
let sigma_portfolio = dotValue > 0 ? sqrt(dotValue) : 0.0

print("Ваги акцій у портфелі мінімального ризику:")
for i in 0..<4 {
    print("A\(i+1): \(String(format: "%.4f", w[i]))")
}

print("\nОчікувана норма прибутку портфеля: \(String(format: "%.4f", E_portfolio))")
print("Ризик портфеля: \(String(format: "%.4f", sigma_portfolio))")
```
# Result:
<img width="465" height="182" alt="Знімок екрана 2025-10-26 о 3 37 35 пп" src="https://github.com/user-attachments/assets/541d81a0-919f-4875-8d5b-a5f92c777d19" />

