![[Pasted image 20240509233456.png]]

Had useCallback not been used this would have given an infinite loop. Re-rendering fetchData would have triggered fetchData again.

useCallback ensures that fetchData is only rendred once when mounted and is not re-rendered => useEffect is also only called once.