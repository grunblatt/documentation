
https://kallahboutique.evelt.app

---

/kallah

<br>
<br>

POST
/addKallah

Body
```json
{
    "name": "perl2",
    "slug": "perl2",
    "volunteer": "rachel",
    "done": false
}
```
<br>

GET
/

<br>

GET
/:slug

<br>

PUT
/editKallah/:slug

Body
```json
{
    "name": "perl2",
    "slug": "perl2",
    "volunteer": "rachel",
    "done": false
}
```
---

/items

<br>
<br>

POST
/addItem
```json
{
    "name": "Bath Towels ",
    "categoryId": 2
}
```
<br>

GET
/

<br>

GET
/:itemId

<br>

PUT
/:itemId
```json
{
    "name": "Bath Towels ",
    "categoryId": 2
}

===


