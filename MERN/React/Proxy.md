```js
// https://vitejs.dev/config/
export default defineConfig({
  server: {
    proxy: {
      'api': {
        target: 'http://localhost:3000',
        secure: false,
      }
    }
  },
  
  plugins: [react()],
})
```

![[Pasted image 20240524145345.png]]