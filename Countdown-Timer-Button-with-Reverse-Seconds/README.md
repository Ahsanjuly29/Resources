# Resources


```markdown
# Countdown Timer Button with Reverse Seconds

This documentation provides a step-by-step guide on how to implement a button with a countdown timer. The button will display a live countdown in minutes and seconds, showing how much time is left before it can be clicked again. After clicking, the button will be disabled and show a countdown until the cooldown period ends.

---

## Features

- Displays the countdown in **minutes and seconds**.
- **Prevents multiple clicks** during the cooldown period.
- **Live countdown** updates every second.
- Button is **disabled** while the timer is active and is re-enabled once the cooldown is finished.

---

## Prerequisites

Before you begin, make sure you have:
- A basic understanding of **HTML**, **JavaScript**, and **jQuery**.
- **jQuery** included in your project.

To include **jQuery** in your HTML file, use the following CDN:

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
```

---

## Steps to Implement

Follow these steps to implement the countdown timer button:

### 1. Create the HTML Structure

Start by creating the basic HTML structure. Add the following button element to your HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Countdown Timer</title>
    <!-- Add your CSS files here -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container mt-5">
        <button id="refreshButton" class="btn btn-success">Click to refresh</button>
    </div>

    <!-- Add your JS scripts here -->
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="main.js"></script>
</body>
</html>
```

### 2. Add the JavaScript Code

Now, add the following JavaScript code to handle the countdown timer functionality. You can place this in a separate `main.js` file, or directly within a `<script>` tag at the bottom of your HTML file.

```javascript
var timer = 4; // Timer in hours (adjust as needed)
var lastClickTime = localStorage.getItem('lastClickTime'); // Get the last click time from localStorage

function updateButton() {
    var currentTime = new Date().getTime();
    var timeLeft = timer * 60 * 60 * 1000 - (currentTime - lastClickTime);

    if (timeLeft > 0) {
        // Disable the button and show the countdown in seconds
        var secondsLeft = Math.ceil(timeLeft / 1000); // Convert milliseconds to seconds
        var minutes = Math.floor(secondsLeft / 60);
        var seconds = secondsLeft % 60;
        
        $('#refreshButton').css({
            'pointer-events': 'none', // Disable clicking
            'opacity': '0.5' // Make it look disabled
        }).text('Please wait ' + minutes + 'm ' + seconds + 's before clicking again');
        $('#refreshButton').addClass('btn-warning', true);
        $('#refreshButton').removeClass('btn-success', true);

        // Set up a live countdown timer
        var countdownInterval = setInterval(function() {
            timeLeft -= 1000; // Decrease 1 second
            secondsLeft = Math.ceil(timeLeft / 1000);
            minutes = Math.floor(secondsLeft / 60);
            seconds = secondsLeft % 60;

            $('#refreshButton').text('Please wait ' + minutes + 'm ' + seconds + 's before clicking again');

            if (timeLeft <= 0) {
                clearInterval(countdownInterval); // Stop the countdown when time is up
                updateButton(); // Re-enable the button once the cooldown period has ended
            }
        }, 1000); // Update every second
    } else {
        // If the time is over, re-enable the button
        $('#refreshButton').css({
            'pointer-events': 'auto', // Enable clicking
            'opacity': '1' // Make it look enabled
        }).text('Click to refresh');
        $('#refreshButton').addClass('btn-success', true);
        $('#refreshButton').removeClass('btn-warning', true);
    }
}

// Call updateButton on page load
$(document).ready(function() {
    if (lastClickTime) {
        updateButton();
    }

    $('#refreshButton').click(function(event) {
        var currentTime = new Date().getTime();
        localStorage.setItem('lastClickTime', currentTime); // Save the current click time to localStorage

        // Enable the button after the click and proceed with the action
        updateButton();
    });
});
```

### 3. Explanation of the Code

- **`timer` Variable**:  
  Defines the cooldown period in hours (set to 4 hours in this example). You can change this based on your requirements.

- **`localStorage`**:  
  Uses `localStorage` to store the time of the last click. This allows the cooldown to persist even after a page refresh.

- **`updateButton` Function**:  
  This function calculates the time remaining in the cooldown period, updates the button’s text, and disables or enables the button. If the cooldown period is still active, the countdown is displayed in minutes and seconds.

- **Countdown Timer**:  
  The `setInterval` function updates the countdown every second and adjusts the button text to show the time remaining.

- **Button Click Event**:  
  When the button is clicked, the current timestamp is saved in `localStorage`, and the `updateButton` function is called to update the button state and start the cooldown.

---

## Customizing the Timer

- **Change the Timer Duration**:  
  Modify the `timer` variable to change the cooldown duration (in hours). For example, to set a 2-hour cooldown:
  ```javascript
  var timer = 2; // 2-hour cooldown
  ```

- **Change Button Styles**:  
  You can customize the button's appearance by adding your own CSS styles or changing the Bootstrap classes (`btn-success`, `btn-warning`).

  Example of custom CSS:
  ```css
  #refreshButton {
      font-size: 16px;
      padding: 10px 20px;
  }
  ```

---

## Testing the Timer

1. Open your HTML file in a web browser.
2. Click the **"Click to refresh"** button.
3. The button will become disabled and display a countdown in minutes and seconds.
4. Wait for the cooldown period to finish, or adjust the timer for quicker testing.
5. Once the cooldown ends, the button will become enabled again, and the text will revert to **"Click to refresh"**.

---

## Conclusion

This implementation creates a button with a reverse countdown timer that prevents users from clicking it multiple times within a cooldown period. It features a live countdown in minutes and seconds, and the button is automatically re-enabled once the cooldown expires.

You can easily customize the timer duration and appearance to suit your project's needs. Happy coding!

---

## License

This project is open-source and available under the [MIT License](LICENSE).
```

---

### How to Use This `README.md`:

1. Copy the above content into a `README.md` file.
2. Place it in the root directory of your project.
3. Share it with anyone who needs to implement the countdown timer functionality in their own projects.