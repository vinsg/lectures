---
title: Using ressources
theme: blood
revealOptions:
  transition: 'none'
  controls: false
  progress: true
---
## Handling resources with Use

---

Resources in the Kotlin/JVM standard library

- FileReader
- java.sql.Connection
- InputStream
- etc.

Notes: Ressources in the Kotlin/JVM sdt library need to be managed

---
```Kotlin [1-8|6]
fun countCharInFile(path: String) Int {
	val reader = BufferedReader(FileReader(path))
	try {
		return reader.lineSequence().sumOf { it.length }
	} finally {
		reader.close()
	}
}
```
Notes: Typical java style code. Code is incorrect and buggy

---

But they implement the `closable` interface
![closable](assets/closable.png)

---

Use the `use` keyword!

Notes: Which means we can use the use keyword to automaticaly close them.

---

```Kotlin [1-6|3]
fun countCharInFile(path: String) Int {
	val reader = BufferedReader(FileReader(path))
	reader.use {
		return reader.lineSequence().sumOf { it.lengh }
	}
}
```

---
```Kotlin [2-4]
fun countCharInFile(path: String) Int {
	BufferedReader(FileReader(path)).use { reader -> 
		return reader.lineSequence().sumOf { it.lengh }
	}
}
```
Notes: The receiver is passed as an argument so we can simplify

---
```Kotlin [2-4]
fun countCharInFile(path: String) Int {
    File(path).useLines { lines ->
        return lines.sumOf { it.lenght }
    }
}
```
Notes: Even better we can use the Kotlin Std library with the useLines function

---

```Kotlin [1-5|1]
fun countCharInFile(path: String) Result<Int> = RunCatching {
	File(path).useLines { lines ->
        lines.sumOf { it.length }
    }
}
```
Notes: We can make our code safer by using returning a Result. 
Now the caller has to handle the possible failure states.

---

Usage

```Kotlin
countCharInFileException("file.txt")
	.onSuccess { println("Char count: $it") }
	.onFailure { println(it.message) }
```
Notes: Example usage

---
### Reference

- `Effective Kotlin` by Marcin Moskala
- Kotlin Idoms: [kotlinlang.org/docs/idioms.html](https://kotlinlang.org/docs/idioms.html#java-7-s-try-with-resources)

---

## Thank you

- @vinsg
- Website: [vinsg.ca](https://vinsg.ca)
- Presentation: [Source]()


