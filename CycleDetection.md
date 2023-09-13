# Алгоритм обнаружения цикла (Флойда, "Заяц и Черепаха")

[Задача Song на TestDome](https://www.testdome.com/library?page=1&term=Song&termMatchType=equalsCaseInsensitive&questionId=88835&skillArea=63)

Алгоритм Флойда, также известный как алгоритм "Заяц и Черепаха", используется для обнаружения циклов в последовательностях или связных структурах, таких как связанные списки. Этот алгоритм был разработан Робертом Флойдом и является частным случаем алгоритма обнаружения циклов.

## Работа алгоритма

1. Используются два указателя: один двигается в два раза быстрее другого.
2. Если есть цикл, быстрый указатель ("заяц") догонит медленный указатель ("черепаху").

**Сложность по времени:** \(O(n)\), где \(n\) - число элементов в последовательности.
**Сложность по пространству:** \(O(1)\), так как используются только два указателя.

## Пример на JavaScript

```javascript
class Song {
  name;
  nextSong;
  
  constructor(name) {
    this.name = name;
  }
  
  /**
   * @return {boolean} true if the playlist is repeating, false if not.
   */
  isInRepeatingPlaylist() {
    let tortoise = this;
    let hare = this;
    
    
    while (hare !== undefined && hare.nextSong !== undefined) {
      tortoise = tortoise.nextSong;           // Move tortoise by one step
      hare = hare.nextSong.nextSong;          // Move hare by two steps
      
      if (tortoise === hare) {                // If they meet, then a loop exists
        return true;
      }
    }
    
    return false;                             // If hare reaches the end, then no loop
  }

}

let first = new Song("Hello");
let second = new Song("Eye of the tiger");

first.nextSong = second;
second.nextSong = first;

console.log(first.isInRepeatingPlaylist());
