### filter

```swift
import Foundation
import Combine

let numbers = (1...20).publisher

numbers.filter { $0 % 2 == 0}
.sink {
    print($0)
}
```

### removeDuplicates

```swift
import Foundation
import Combine

let words = "apple apple fruit apple mango watermelon apple".components(separatedBy: " ").publisher

words
    .removeDuplicates()
    .sink {
    print($0)
}
```

### compactMap

```swift
import Foundation
import Combine

// ignore nil values
let strings = ["a", "1.24", "b", "3.45", "6.7"]
    .publisher
    .compactMap {
        Float($0)
    }
    .sink {
        print($0)
    }
```

### ignoreOutput

```swift
import Foundation
import Combine

let numbers = (1...5000).publisher

// ignore all the outputs of values in the sequence
numbers
    .ignoreOutput()
    .sink(receiveCompletion: { print($0) }, receiveValue: { print($0)})
```

### first

```swift
import Foundation
import Combine

let numbers = (1...9).publisher

// find fist number which matches some condition
numbers.first(where: {$0 % 2 == 0})
    .sink {
        print($0)
    }
```

### last

```swift
import Foundation
import Combine

let numbers = (1...9).publisher

numbers.last(where: { $0 % 2 == 0 })
    .sink {
        print($0)
    }
```

### dropFirst

```swift
import Foundation
import Combine

let numbers = (1...9).publisher

numbers.dropFirst(8)
    .sink {
        print($0) // output: 9, drop된 element의 다음 element
    }
```

### dropWhile

```swift
import Foundation
import Combine

let numbers = (1...10).publisher

// drop everything before a certain condition is met
numbers.drop(while: { $0 % 3 != 0 })
    .sink {
        print($0)
    }
```

### dropUntilOutputfrom

- If another publisher publishes a value based on that, it can let go of the barrier and then all the other values kind of like start passing through.

```swift
import Foundation
import Combine

let isReady = PassthroughSubject<Void, Never>()
let taps = PassthroughSubject<Int, Never>()

taps.drop(untilOutputFrom: isReady).sink {
    print($0)
}

(1...10).forEach { n in
    taps.send(n)

    if n == 3 {
        isReady.send()
    }

}
```

### prefix

```swift
import Foundation
import Combine

let numbers = (1...10).publisher

numbers
    .prefix(2)
    .sink {
    print($0) // 1, 2
}

numbers
    .prefix(while: { $0 < 3 })
    .sink {
        print($0) // 1, 2
    }
```

### Exercise

```swift
import Foundation
import Combine

// 1. Skip the first 50 values emitted by the upstream publisher
// 2. Take the next 20 values after those first 50 values
// 3. only take even numbers

let numbers = (1...100).publisher

numbers
    .dropFirst(50)
    .prefix(20)
    .filter { $0 % 2 == 0 }
    .sink {
        print($0)
    }
```
