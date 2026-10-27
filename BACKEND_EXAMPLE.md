# Optional Node.js Backend Example

This backend uses the same logic as your Python code.

Your Python logic:

```python
if 'height' in r.text and 'width' in r.text:
    return LIVE
else:
    return DIE
```

Node.js version:

```js
import express from 'express'
import cors from 'cors'
import rateLimit from 'express-rate-limit'

const app = express()
app.use(cors())
app.use(express.json())

app.use(rateLimit({
  windowMs: 60 * 1000,
  max: 60
}))

app.post('/api/check-uids', async (req, res) => {
  const uids = Array.isArray(req.body.uids) ? req.body.uids.slice(0, 500) : []

  const results = await Promise.all(
    uids.map(async uid => {
      const url = `https://graph2.facebook.com/v3.3/${uid}/picture?redirect=0`

      try {
        const response = await fetch(url)
        const text = await response.text()
        const lowerText = text.toLowerCase()

        const live = lowerText.includes('height') && lowerText.includes('width')

        return {
          uid,
          status: live ? 'live' : 'dead',
          reason: live
            ? 'LIVE - Facebook account is active'
            : 'DIE - Facebook account not found or deactivated',
          httpStatus: response.status
        }
      } catch (error) {
        return {
          uid,
          status: 'error',
          reason: error.message
        }
      }
    })
  )

  res.json(results)
})

app.listen(3000, () => console.log('API running on port 3000'))
```

## Full backend

A full working backend is included in:

```txt
backend/server.js
```
