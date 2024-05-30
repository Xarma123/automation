// ==UserScript==
// @name         Auto Click Button at Specified Time
// @namespace    http://tampermonkey.net/
// @version      1.3
// @description  Automatically click a button at the time specified in a div on the webpage, reduced by 1 minute, with dynamic time monitoring
// @author       Your Name
// @match        https://app.outlier.ai/en/expert/tasks
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
       function playSound() {
        let sound = new Audio("https://audio.jukehost.co.uk/zi1hmurT8g7DCM1o63UXMtAAHTcY6A77"); // Change the file path to your audio file
        sound.play();
       }
      function addControls() {
        const controlsContainer = document.createElement('div');
        controlsContainer.style.position = 'fixed';
        controlsContainer.style.bottom = '10px';
        controlsContainer.style.left = '10px';
        controlsContainer.style.zIndex = '1000';
        controlsContainer.style.backgroundColor = 'white';
        controlsContainer.style.border = '1px solid #ccc';
        controlsContainer.style.padding = '10px';
        controlsContainer.style.borderRadius = '5px';

        controlsContainer.innerHTML = `
            <div>
                <label><input type="radio" name="buttonSelect" value="button1"> Approve with Changes</label>
            </div>
            <div>
                <label><input type="radio" name="buttonSelect" value="button2"> Send back to queue</label>
            </div>
            <div>
                <label><input type="radio" name="buttonSelect" value="none" checked> No Button</label>
            </div>
        `;

        document.body.appendChild(controlsContainer);
    }
       function getSelectedButton() {
        const selectedValue = document.querySelector('input[name="buttonSelect"]:checked').value;
        if (selectedValue === 'button1') {
            return document.querySelector('button.scaleui.inline-flex.items-center.rounded.border-0.font-medium.shadow-sm.focus\\:outline-none.focus\\:ring-2.focus\\:ring-offset-2.transition-all.px-2\\.5.h-7.text-xs.text-danger-800.bg-danger-100.hover\\:bg-danger-200.focus\\:ring-danger-100'); // Selector for "Send Back To Queue"
        } else if (selectedValue === 'button2') {
            return document.querySelector('button.scaleui.inline-flex.items-center.rounded.border-0.font-medium.shadow-sm.focus\\:outline-none.focus\\:ring-2.focus\\:ring-offset-2.transition-all.px-2\\.5.h-7.text-xs.text-success-800.bg-success-100.hover\\:bg-success-200.focus\\:ring-success-100'); // Selector for "Approve With Changes"
        } else {
            return null;
        }
    }


    // Function to parse the time from the string (format: "Wednesday, 8:00 PM GMT+5:30")
    function parseTime(timeString) {
        const [day, timePart] = timeString.split(',').map(part => part.trim());
        const [time, period, timezone] = timePart.split(' ');

        let [hours, minutes] = time.split(':').map(Number);
        if (period === 'PM' && hours < 12) hours += 12;
        if (period === 'AM' && hours === 12) hours = 0;

        const offset = timezone.match(/GMT([+-]\d+:\d+)/)[1];
        const [offsetHours, offsetMinutes] = offset.split(':').map(Number);
        const totalOffsetMinutes = offsetHours * 60 + (offsetHours < 0 ? -offsetMinutes : offsetMinutes);

        const now = new Date();
        const targetTime = new Date();
        targetTime.setUTCHours(hours, minutes - totalOffsetMinutes, 0, 0);

        // Adjust for the specific day of the week
        const daysOfWeek = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
        const targetDay = daysOfWeek.indexOf(day);
        const currentDay = now.getUTCDay();

        const dayDifference = (targetDay - currentDay + 7) % 7;
        targetTime.setUTCDate(now.getUTCDate() + dayDifference);

        return targetTime;
    }

    // Function to calculate the delay until the target time in milliseconds, reduced by 1 minute
    function calculateDelay(targetTime) {
        const now = new Date();
        const oneMinute = 60 * 1000; // 1 minute in milliseconds
        return (targetTime - now) - oneMinute;
    }

    // Function to click the button at the specified time
    function scheduleButtonClick(timeString) {
        const targetTime = parseTime(timeString);

        if (targetTime) {
            const delay = calculateDelay(targetTime);


            console.log(`Button will be clicked at ${targetTime}, which is in ${(delay + 60 * 1000) / 1000} seconds (adjusted by -1 minute)`);


            // Set a timeout to click the button at the target time, reduced by 1 minute
            setTimeout(() => {
                  playSound();
                const button = getSelectedButton();
                if (button) {
                    button.click();
                    console.log('Button clicked');

                } else {
                    console.log('Button not found');

                }
                 setTimeout(() => {
                        observeElement('div.flex.items-start', onTimeDivChange);
                    }, 2000);
            }, delay);
        } else {
            console.log('Invalid time format');
        }
    }

    // Function to observe changes in the specified element
    function observeElement(selector, callback) {
        const targetNode = document.querySelector(selector);

        if (targetNode) {
            const config = { childList: true, subtree: true, characterData: true };

            const observer = new MutationObserver((mutationsList) => {
                for (let mutation of mutationsList) {
                    if (mutation.type === 'childList' || mutation.type === 'characterData') {
                        callback(targetNode);
                    }
                }
            });

            observer.observe(targetNode, config);

            // Initial callback execution to handle the initial state
            callback(targetNode);
        } else {
            console.log('Target node not found');
            setTimeout(() => {
                        observeElement('div.flex.items-start', onTimeDivChange);
                    }, 2000);
        }
    }

    // Function to handle the time div changes
    function onTimeDivChange(node) {
        const timeString = node.textContent.trim();
        console.log('Time div updated:', timeString);
        document.querySelector('input[name="buttonSelect"][value="none"]').checked = true;

        scheduleButtonClick(timeString);
    }

            addControls()
            observeElement('div.flex.items-start', onTimeDivChange);


})();
