---
title: Flatten nested objects
shortTitle: Flatten nested objects
language: javascript
tags: [object, array]
cover: false
excerpt: Flatten a deeply nested object into a single-level object with dot-separated keys.
listed: true
dateModified: 2025-12-09
---

Flatten a deeply nested object into a single-level object with dot-separated keys.

// Flatten nested objects function
function flattenObject(obj, prefix = '', res = {}) {
  for (const key in obj) {
    const propName = prefix ? `${prefix}.${key}` : key;
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      flattenObject(obj[key], propName, res);
    } else {
      res[propName] = obj[key];
    }
  }
  return res;
}

// Example usage
const nested = {
  a: {
    b: {
      c: 1
    },
    d: 2
  },
  e: 3
};

console.log(flattenObject(nested));
// Output: { "a.b.c": 1, "a.d": 2, "e": 3 }
