![[Pasted image 20240816224752.png]]

When making, i was having the issue of buffers library and stuff, the way for that is, JavaScript uses uint8 array, but react and the libraries used in the project used buffers, slightly different, so make sure it is there,  by doing `npm install vite-plugin-node-polyfills`
and then `import { nodePolyfills } from 'vite-plugin-node-polyfills'` 



[[JSON RPC]]
[[Public and Private key pairs -]]
