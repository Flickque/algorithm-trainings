# Event Delegation Optimization

## Objective
[Reorder Tasks TestDome](https://www.testdome.com/library?page=1&skillArea=31&questionId=102244)


Benefits of event delegation:

1. Fewer handlers: Instead of multiple handlers, one handler manages the event at a higher level.
2. Dynamically added elements: If elements are dynamically added or removed (e.g., via AJAX), you don't need to add or remove handlers for each new element. The handler set on the parent will automatically "listen" to new elements.
3. Efficiency: It can improve performance, especially when there are many elements and/or frequent changes in the DOM.

## Key methods

classList.contains
event.target.closest
"div".nextElementSibling
"div".previousElementSibling

## Given Code

```javascript
function initialize() {
  document.addEventListener('click', (event) => {
      if (event.target.classList.contains('downButton') || event.target.classList.contains('upButton')) {
          let siblingDiv;
          const currentDiv = event.target.closest('div');

          if (event.target.classList.contains('downButton')) {
              siblingDiv = currentDiv.nextElementSibling;
          } else if (event.target.classList.contains('upButton')) {
              siblingDiv = currentDiv.previousElementSibling;
          }

          if (siblingDiv) {
              const currentSpan = currentDiv.querySelector('span');
              const siblingSpan = siblingDiv.querySelector('span');
              [currentSpan.textContent, siblingSpan.textContent] = [siblingSpan.textContent, currentSpan.textContent];
          }
      }
  });
}

document.body.innerHTML = `
<div>
    <span>Read emails</span>
    <button class="downButton" type="button">↓</button>
</div>
<div>
    <span>Prepare presentation</span>
    <button class="downButton" type="button">↓</button>
    <button class="upButton" type="button">↑</button></div>
<div>
    <span>Monthly report</span>
    <button class="upButton" type="button">↑</button>
</div>`;

initialize();

document.querySelectorAll("button")[0].click();
document.querySelectorAll("button")[3].click();
document.querySelectorAll("button")[1].click();

console.log(document.body.innerHTML);
