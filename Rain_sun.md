# laboratory 1

```swift
func readDouble(prompt: String) -> Double {
        print(prompt, terminator: "")
        if let input = readLine(), let value = Double(input) {
            return value
        }else {
            print("Непраивильно введені дані, щодо доща, спробуйте ще раз")
            return readDouble(prompt: prompt)
        }
    }

func readInt(prompt: String) -> Int {
        print(prompt, terminator: "")
        if let input = readLine(), let value = Int(input) {
            return value
        }else {
            print("Непраивильно введені дані, спробуйте ще раз")
            return readInt(prompt: prompt)
        }
    }

        func expectedUtility(probRain: Double, rainScore: Int, sunScore: Int) -> Double {
            return probRain * Double(rainScore) + (1 - probRain) * Double(sunScore)
        }
        
        let raitRain: Double = readDouble(prompt: "Введіть ймоврінсть ДОЩУ від 0 до 1:")
        
        let homeRain: Int = readInt(prompt: "Введіть оцінку ДОЩОВОЇ погоди, коли Ви ВДОМА від 1 до 10: ")
        let homeSun: Int = readInt(prompt: "Введіть оцінку СОНЯЧНОЇ погоди, коли Ви ВДОМА від 1 до 10: ")
        let outRain: Int = readInt(prompt: "Введіть оцінку ДОЩОВОЇ погоди, коли Ви на ВУЛИЦІ від 1 до 10: ")
        let outSun: Int = readInt(prompt: "Введіть оцінку СОНЯЧНОЇ погоди, коли Ви на ВУЛИЦІ від 1 до 10: ")
        
        let userHome = expectedUtility(probRain: raitRain, rainScore: homeRain, sunScore: homeSun)
        let userOut = expectedUtility(probRain: raitRain, rainScore: outRain, sunScore: outSun)
        
        print("\nРезультати:")
        print("Очікувана корисність для 'сидіти вдома': \(userHome)")
        print("Очікувана корисність для 'вийти на вулицю': \(userOut)")
        
        if userOut > userHome {
            print("Більше корисно, вийти на вулицю")
        } else if userOut < userHome {
            print("Більше корисно, сидіти вдома")
        } else if userOut == userHome {
            print("Зробіть власний вибір)")
        }


```
<img width="557" height="221" alt="ScreenShot" src="https://github.com/user-attachments/assets/00c4b429-cec3-4773-8122-e5a803224241" />


