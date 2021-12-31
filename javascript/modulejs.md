# Module JS

## __dirname replacement

```javascript
const __dirname = new URL('.', import.meta.url).pathname;
```
