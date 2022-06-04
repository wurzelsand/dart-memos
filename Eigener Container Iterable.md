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

Die Werte sollen in einer Klasse `Node` gespeichert werden, die auch eine Verknüpfung zum nächsten Objekt (`nextNode`) bereit stellt:

```dart
class Node<T> {
  final T value;
  Node<T>? nextNode;

  Node(this.value);
}
```

Die Klasse `MyQueueIterator` soll aus einem «Start»-`Node` einen Iterator erzeugen:

```dart
class MyQueueIterator<T> implements Iterator<T> {
  final Node<T> firstNode;
  Node<T>? ptr;

  MyQueueIterator(this.firstNode);

  @override
  T get current => ...

  @override
  bool moveNext() {
    ...
  }
}
```

Die Klasse `MyQueue` soll schließlich mit Hilfe von `MyQueueIterator` die Klasse `Iterable` erweitern, so dass die `for-in`-Schleife möglich wird:

```dart
class MyQueue<T> extends Iterable<T> {
  final Node<T> firstNode;

  MyQueue(T value) ...

  void append(T value) ...

  @override
  Iterator<T> get iterator ...
}
```

## Ausführung

```dart
class Node<T> {
  final T value;
  Node<T>? nextNode;

  Node(this.value);
}

extension AppendNode<T> on Node<T> {
  void addAfterEnd(T value) {
    if (nextNode == null) {
      nextNode = Node(value);
    } else {
      nextNode!.addAfterEnd(value);
    }
  }
}

class MyQueueIterator<T> implements Iterator<T> {
  final Node<T> firstNode;
  Node<T>? ptr;

  MyQueueIterator(this.firstNode);

  @override
  T get current => ptr!.value;

  @override
  bool moveNext() {
    if (ptr == null) {
      ptr = firstNode;
    } else {
      ptr = ptr!.nextNode;
    }
    return ptr != null;
  }
}

class MyQueue<T> extends Iterable<T> {
  final Node<T> firstNode;

  MyQueue(T value) : firstNode = Node(value);

  void append(T value) {
    firstNode.addAfterEnd(value);
  }

  @override
  Iterator<T> get iterator => MyQueueIterator(firstNode);
}

void main() {
  var queue = MyQueue(1);
  queue.append(2);
  queue.append(3);

  for (final value in queue) {
    print(value);
  }
}
```
