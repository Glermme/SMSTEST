# Vercel + Python + Supabase + SMS form handler

This project gives you a Vercel-deployable Python backend that:

- accepts form submissions
- stores them in Supabase
- texts the submitter a confirmation
- texts the business owner about the new lead

## Files

- `api/index.py` - Flask API entrypoint for Vercel
- `requirements.txt` - Python dependencies
- `vercel.json` - routes all requests to the Python function
- `.env.example` - environment variables you must set in Vercel
- `schema.sql` - SQL to create the Supabase table


Env Example

# Supabase
SUPABASE_URL=https://your-project-id.supabase.co
SUPABASE_KEY=your-supabase-service-role-key

# Twilio
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_PHONE_NUMBER=+15551234567

# Business owner number (receives alerts)
BUSINESS_OWNER_PHONE=+15557654321






## Deploy steps

1. Upload these files to your GitHub repo.
2. In Supabase, run `schema.sql` in the SQL editor.
3. In Vercel, import the GitHub repo.
4. In Vercel project settings, add all variables from `.env.example`.
5. Deploy.


## Included frontend

A ready-to-use frontend file is included:

- `form.html` - styled HTML form that submits to the same deployment's root URL

After deployment, open:

- `https://your-project.vercel.app/form.html`

The form uses JavaScript `fetch('/')`, so it is already wired to the backend in this repo.

## Form POST example

Send a `POST` request to your deployed root URL with JSON like:

```json
{
  "name": "Jane Smith",
  "phone": "+1234567890",
  "email": "jane@example.com",
  "message": "I want a quote"
}
```

## Basic frontend example

```html
<form id="contactForm">
  <input type="text" name="name" placeholder="Name" required>
  <input type="tel" name="phone" placeholder="Phone" required>
  <input type="email" name="email" placeholder="Email">
  <textarea name="message" placeholder="Message"></textarea>
  <button type="submit">Submit</button>
</form>

<script>
  document.getElementById('contactForm').addEventListener('submit', async (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const payload = Object.fromEntries(formData.entries());

    const response = await fetch('/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });

    const result = await response.json();
    alert(result.message || result.error);
  });
</script>
```

## Notes

- Use E.164 phone format like `+1234567890`.
- If you use a Twilio trial account, only verified numbers may be able to receive messages.
- Store your service role key only in server-side environment variables, never in browser code.
