The PUT method in REST API is used to update an existing resource on the server. When a client sends a PUT request to a specific URL that represents a resource, the server should update the entire resource with the data provided in the request body, replacing the existing data.

In the provided code, the `app.put('/api/people/:id')` route is responsible for handling PUT requests to update a person's name based on their ID.
```js
app.put('/api/people/:id', (req, res) => {
    const { id } = req.params;
    const { name } = req.body;

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.map((p) => {
            if (p.id === parseInt(id))
                p.name = name
            return p
        })
        res.status(200).json({ success: true, data: newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})
```
Here's what the code does:

1. The route `/api/people/:id` is defined with the `app.put` method, indicating that it will handle PUT requests to this URL.

2. The `id` parameter is extracted from the URL using `req.params.id`.

3. The `name` property is extracted from the request body using `req.body.name`.

4. The existing `person` object is found in the `people` array by searching for an object with an `id` that matches the provided `id` parameter.

5. If the `person` is found:
   a. A new array `newPeople` is created by mapping over the `people` array.
   b. For each object in the `people` array, if the `id` matches the provided `id`, the `name` property is updated with the new `name` from the request body.
   c. The updated `newPeople` array is sent as the response with a 200 status code, indicating success.

6. If the `person` is not found, a 404 status code is sent with an error message indicating that no person with the provided `id` exists.

In summary, the PUT method is used to update an existing resource by replacing it entirely with the new data provided in the request body. In this case, the code updates the name of a person identified by their `id` in the `people` array.