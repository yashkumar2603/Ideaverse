There is a library called React SWR and it does the same thing of providing implementations of custom hooks in a library format.
[React Hooks for Data Fetching – SWR](https://swr.vercel.app/)
# useFetch() hook-
**Used to fetch data from a backend server for example**
```typescript
import { useState, useEffect } from 'react';

export const useFetch = (url: string) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err as Error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};
```

### useFetch() with re-fetching on interval 
```typescript
import { useState, useEffect } from 'react';

export const useFetch = (url: string, interval: number | null = null) => {
  const [data, setData] = useState<any>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<Error | null>(null);

  const fetchData = async () => {
    setLoading(true);
    try {
      const response = await fetch(url);
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData(); // Initial fetch

    if (interval !== null) {
      const fetchInterval = setInterval(() => {
        fetchData();
      }, interval);

      return () => clearInterval(fetchInterval); // Clear interval on cleanup
    }
  }, [url, interval]);

  return { data, loading, error };
};
```

This fetches repeatedly at the interval end and updates the data accordingly
We can keep this in a separate folder of .js files and export as functions, then import these functions to use directly in the component.

# useDebounce() hook -
![[Pasted image 20241212161723.png]]
We protect the expensive call with a spam filter that waits for the spam to end and then calls the backend after 30 secs of spamming ending.
Debounce function 
```javascript
const debounce = (func, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);  // Clears the previous timer
    timeout = setTimeout(() => func(...args), delay);  // Starts a new timer
  };
};
```

Debounce hook 
```typescript
import { useState, useEffect } from 'react';

const useDebounce = (value, delay) => {
    const [debouncedValue, setDebouncedValue] = useState(value);

    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);

        return () => {
            clearTimeout(handler);
        };
    }, [value]);

    return debouncedValue;
};

export default useDebounce;
```
