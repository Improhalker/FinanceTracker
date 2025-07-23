# 💰 Finance Tracker

A lightweight finance tracker that helps you keep an eye on **incoming funds** and **outgoing investments** in your wallet.

It provides a clear overview of your financial status and is built for fast development and flexibility.

---

## 🛠️ Tech Stack

- **Frontend**: [Nuxt 3](https://nuxt.com/)
- **Validation**: [Zod](https://zod.dev/)
- **Data Generation (dev only)**: [@faker-js/faker](https://github.com/faker-js/faker)
- **Date utils**: [date-fns](https://date-fns.org/)
- **Backend / DB**: [Supabase](https://supabase.com/)

---

## ⚠️ Status

This app depends on a **Supabase** instance, which:
- Requires **weekly activity** on the free tier
- Can become **unavailable after repeated restores**

> 📛 Supabase is currently **disconnected** due to these limitations.

---

## 📦 Features

- ✅ Form validation with Zod  
- 📥 Track incoming transactions  
- 📤 Track outgoing investments  
- 📆 Smart date formatting with `date-fns`  
- 🧪 Seed database with fake data (dev only)

---

## 🚧 TODO

- [ ] Replace or stabilize Supabase connection  
- [ ] Add user authentication (optional)  
- [ ] Add charts / graphs for trends  
- [ ] Export reports / summaries  
- [ ] Mobile improvements  

---

## 🧪 Development

```bash
# Install dependencies
npm install

# Run the app
npm run dev
