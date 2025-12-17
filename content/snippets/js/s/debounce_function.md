---
title: Debounce function
shortTitle: Debounce function
language: javascript
tags: [function, utility, debounce]
cover: false
excerpt: Create a debounce function to limit the rate at which a function can fire.
listed: true
dateModified: 2025-12-17
---

Create a debounce function to limit the rate at which a function can fire.

// Debounce function
function debounce(func, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Example usage
const log = debounce((msg) => console.log(msg), 500);

log("Hello");
log("Hello again"); // Only this one will be logged after 500ms
