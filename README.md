### SWR
Alpine fetch hook example, based on SWR library

### ⚡️ Implementation
```js
const useSWR = (key, callback, options = {
    isLoadingKey: `isLoading`,
    dataKey: `data`,
    errorKey: `error`,
    mutationKey: `mutate`
}) => {
    Alpine?.data(key, () => ({
        init(){
            this.mutate()
        },
        [options?.isLoading || 'isLoading']: true,
        [options?.dataKey || 'data']: null,
        [options?.errorKey || 'error']: null,
        async [options?.mutationKey || 'mutate']() {
            if(!key) return;

            if(!callback) {
                console.warn(`callback for ${key} hook is invalid: ${typeof callback}`)
                return
            }

            this.isLoading = true;
            try {
                this.data = await callback()
            }catch(error){
                this.error = error
            }
            this.isLoading = false;
        }
    }));               
}
```