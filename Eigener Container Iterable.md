# Eigener Container Iterable

## Aufgabe

Baue eine Klasse `MyQueue`. Sie soll als Container für Werte eines beliebigen Typs dienen, z. B. `int`:

```dart
void main() {
  var queue = MyQueue(1);
  queue.append(2);
  queue.append(3);

  for (final value in queue) {
    print(value); // 1 2 3
  }
}
```
