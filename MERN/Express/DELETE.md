The DELETE method is commonly used in RESTful APIs to delete or remove a resource from the server, such as deleting a user account, removing a blog post, or deleting a product from an e-commerce platform.
DELETE doesn't expect a body and if provided it is ignored.

```js
app.delete('/api/people/:id', (req, res) => {
    const { id } = req.params

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.filter((p) => p.id !== person.id)
        res.status(200).json({ success: true, newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})
```

In the provided code, the `app.delete('/api/people/:id', (req, res) => { ... })` route handler is responsible for handling the DELETE request for removing a person from the `people` array based on their `id`.

Here's a breakdown of the code inside the `app.delete` route handler:

1. `const { id } = req.params` retrieves the `id` parameter from the URL path (e.g., `/api/people/3`).

2. `const person = people.find((p) => p.id === parseInt(id))` finds the person object in the `people` array that matches the provided `id`.

3. If the `person` is found:
   - `const newPeople = people.filter((p) => p.id !== person.id)` creates a new array `newPeople` that contains all the people except the one with the matching `id`.
   - `res.status(200).json({ success: true, newPeople })` sends a successful response with a status code of 200 and includes the updated `newPeople` array in the response body.

4. If the `person` is not found:
   - `res.status(404).json({ success: false, msg: `no person with id ${id}` })` sends a response with a status code of 404 (Not Found) and includes an error message indicating that no person with the given `id` was found.

So, when a client sends a DELETE request to the `/api/people/:id` endpoint with a specific `id`, this route handler will remove the person with that `id` from the `people` array and return the updated array in the response. If no person with the provided `id` is found, it will return a 404 Not Found error.