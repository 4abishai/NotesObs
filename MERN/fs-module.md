## Sync

```js
const { readFileSync, writeFileSync } = require('fs')
console.log("start")

const filea = readFileSync("./content/a.txt", "utf8")
const fileb = readFileSync("./content/b.txt", "utf8")

console.log(filea)
console.log(fileb)

writeFileSync(
    "./content/c.txt",
    `This is obtained after concatenating filea and fileb:-\n${filea} ${fileb}`
)

// writeFileSync(
//     "./content/c.txt",
//     `This is obtained after concatenating filea and fileb:-\n${filea} ${fileb}`,
//     { flag: 'a' }    // append
// )


const filec = readFileSync("./content/c.txt", "utf8")
console.log(filec)
```

## A-sync

```js
const { readFile, writeFile } = require('fs')
console.log("start")

readFile("./content/a.txt", "utf8", (err, response) => {
    if (err) {
        console.log("Some error occured: ", err)
        return
    }

    const filea = response
    console.log(filea)

    readFile("./content/b.txt", "utf8", (err, reponse) => {
        if (err) {
            console.log("Some error occured: ", err)
            return
        }

        const fileb = response
        console.log(reponse)

        writeFile("./content/c.txt",
            `Here is the result : ${filea}, ${fileb}`,
            (err, response) => {
                if (err) {
                    console.log(err)
                    return
                }
                const filec = response
                console.log(filec)
                console.log('done with this task')
            }
        )
    })
})
```

## Note
## 1.
