
```javascript

const minutesInput = document.getElementById("minutesInput");
const secondsInput = document.getElementById("secondsInput");

function parseTimeInput() {
    const inputMinutes = parseInt(minutesInput.value, 10);
    const inputSeconds = parseInt(secondsInput.value, 10);
    if (!isNaN(inputMinutes) && !isNaN(inputSeconds)) {

        if (inputSeconds > 59) {
            seconds = 59;
        } else if (inputSeconds < 0) {
            seconds = 0;
        } else {
            seconds = inputSeconds;
        }

        if (inputMinutes >= 60) {
            minutes = 60;
            seconds = 0;
        } else if (inputMinutes < 0) {
            minutes = 25;
        } else {
            minutes = inputMinutes;
        }
    }
}

```


Link
[[input 태그 설정]]