## 🦀 Rust em Comparação — Guia Visual de Referência Rápida

Este material foi criado para servir como referência rápida para quem já programa em linguagens convencionais (Java, Python, C++) e quer compreender como o Rust aborda conceitos familiares com segurança, performance e previsibilidade.

📘 Objetivo: Apresentar paralelos diretos entre Rust e linguagens tradicionais, com foco prático e visual.

### 🧩 1. Sintaxe Básica

Rust usa uma sintaxe moderna e expressiva, semelhante a C/Java, mas com imutabilidade por padrão e tipagem explícita opcional.

```rust
fn main() {
    let nome = "Elson";      // imutável
    let mut idade = 32;      // mutável
    println!("Olá, {nome}, idade {idade}");
}
```

### 🔒 2. Imutabilidade e Constantes
🧱 Imutável (let)

Por padrão, variáveis são imutáveis:

```rust
let x = 10;
x = 20; // ❌ Erro
```

🔧 Mutável (let mut)
```rust
let mut contador = 0;
contador += 1; // ✅ permitido
```

🧊 Constante (const)

Valores fixos conhecidos em tempo de compilação:

```rust
const LIMITE: i32 = 100;
```

| Característica     | `let` / `let mut` | `const`     |
| ------------------ | ----------------- | ----------- |
| Mutável?           | Sim, com `mut`    | Nunca       |
| Tipo obrigatório?  | Opcional          | Obrigatório |
| Tempo de definição | Execução          | Compilação  |


### 🧱 3. Estruturas e Composição

Rust não usa herança — preferindo composição explícita.

```rust
struct Motor {
    potencia: u32,
}

struct Carro {
    modelo: String,
    motor: Motor,
}
```
💡 Composição substitui herança e torna o ownership claro.


### 🌀 4. Enum, Option e Result
⚙️ Enum em Rust ≠ Enum em Java

```java
enum Direcao { NORTE, SUL, LESTE, OESTE }
```

```rust
enum Direcao {
    Norte,
    Sul,
    Leste,
    Oeste,
    Coordenada(i32, i32),
}
```

Rust usa enums para modelar estados e variações de dados, não apenas valores fixos.

---

**🧩 Option<T> — Substitui null**

```rust
let nome: Option<String> = Some("Elson".to_string());

match nome {
    Some(n) => println!("{}", n),
    None => println!("Sem nome!"),
}
```
---

**⚙️ Result<T, E> — Erros sem exceções**
```rust
let valor: Result<i32, _> = "abc".parse();

match valor {
    Ok(v) => println!("Número: {}", v),
    Err(e) => println!("Erro: {}", e),
}
```
💬 Result torna os erros parte do tipo — sem try/catch.

### 📦 5. Coleções e Iteradores

| Tipo            | Similar em Java | Similar em Python |
| --------------- | --------------- | ----------------- |
| `Vec<T>`        | `List<T>`       | `list`            |
| `HashMap<K, V>` | `Map<K, V>`     | `dict`            |
| `String`        | `String`        | `str`             |


```rust
let numeros = vec![1, 2, 3, 4, 5];

let dobrados: Vec<i32> = numeros
    .iter()
    .filter(|&&n| n % 2 == 0)
    .map(|&n| n * 2)
    .collect();

```

📊 Visual:

```scss
Vec → iter() → filter() → map() → collect()
```

| Método         | Consome dados? | Uso típico    |
| -------------- | -------------- | ------------- |
| `.iter()`      | ❌              | leitura       |
| `.iter_mut()`  | ⚙️             | modificação   |
| `.into_iter()` | ✅              | mover valores |

### ⚠️ 6. Tratamento de Erros

```java
try {
    int n = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    e.printStackTrace();
}
```

```rust
fn parse_numero(s: &str) -> Result<i32, std::num::ParseIntError> {
    s.parse::<i32>()
}

fn main() {
    match parse_numero("abc") {
        Ok(n) => println!("Número: {}", n),
        Err(e) => println!("Erro: {}", e),
    }
}
```

**🔁 Propagando com ?**

```rust
fn processar() -> Result<(), std::io::Error> {
    let conteudo = std::fs::read_to_string("arquivo.txt")?;
    println!("{}", conteudo);
    Ok(())
}
```
? retorna automaticamente o erro para quem chamou.

## 🔄 7. Controle de Fluxo

**if / else**
```rust
let resultado = if x > 5 { "alto" } else { "baixo" };
```

**loop / while / for**
```rust
loop { /* infinito */ }
while condicao { /* enquanto */ }
for x in vec.iter() { /* percorre */ }
```

**match**
```rust
match opcao {
    Some(v) => println!("{}", v),
    None => println!("Nada!"),
}
```

**if let / while let**
```rust
if let Some(v) = opcao { println!("{}", v); }
while let Some(v) = pilha.pop() { println!("{}", v); }

```

## 🔑 8. Ownership e Borrowing

Rust garante segurança de memória sem garbage collector usando três regras:

Cada valor tem um único dono.

Só pode haver um dono ativo por vez.

Quando o dono sai do escopo, o valor é liberado.

🧩 Exemplo: Primitivos
```rust
let a = 10;
let b = a; // cópia simples (Copy)
println!("{a}, {b}");
```

🧱 Exemplo: Objetos
```rust
let s1 = String::from("Olá");
let s2 = s1; // move a posse
println!("{s1}"); // ❌ erro: valor movido
```

💡 Use referência para evitar mover:
```rust
let s1 = String::from("Olá");
let s2 = &s1; // empréstimo (borrow)
println!("{s1}, {s2}");
```

**🧠 Resumo Geral**

| Conceito          | Java / Python         | Rust                              |
| ----------------- | --------------------- | --------------------------------- |
| Mutabilidade      | variável por padrão   | imutável por padrão               |
| Null              | existe                | substituído por `Option<T>`       |
| Exceções          | `try/catch`           | `Result<T, E>`                    |
| Herança           | `extends` / `super`   | composição explícita              |
| Coleções          | `List`, `Map`, `dict` | `Vec`, `HashMap`                  |
| Garbage Collector | sim                   | não (ownership)                   |
| Iteração          | loops                 | `iter().map().filter().collect()` |

--- 

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="card.svg">
    <source media="(prefers-color-scheme: light)" srcset="card.svg">
    <img alt="Documento"
         src="card.svg"
         width="50%">
  </picture>

</div>
