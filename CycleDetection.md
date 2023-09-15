# Floyd's Cycle Detection Algorithm ("Tortoise and the Hare")

[TestDome's Song Task](https://www.testdome.com/library?page=1&term=Song&termMatchType=equalsCaseInsensitive&questionId=88835&skillArea=63)

Floyd's algorithm, also known as the "Tortoise and the Hare" algorithm, is used to detect cycles in sequences or linked structures like linked lists. This algorithm was devised by Robert Floyd and is a special case of the cycle detection algorithm.

## How the Algorithm Works

1. Two pointers are used: one moves twice as fast as the other.
2. If there is a cycle, the fast pointer ("hare") will catch up to the slow pointer ("tortoise").

**Time Complexity:** \(O(n)\), where \(n\) is the number of elements in the sequence.
**Space Complexity:** \(O(1)\), since only two pointers are used.

## JavaScript Example

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
