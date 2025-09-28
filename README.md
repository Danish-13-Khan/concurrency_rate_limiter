Stop Your Go API From Crashing: The Magic of Concurrency Limiter Middleware

This project demonstrates a Concurrency Rate Limiter using Gin in Golang.
It controls how many requests can run at the same time, allows some to wait in a queue, and rejects others if the system is too busy.

📂 Project Structure
concurrency_rate_limiter/
│── config/
│   └── config.go        # Load JSON config
│── middleware/
│   └── concurrency.go   # Middleware logic
│── models/
│   └── config.go        # Config struct
│── config.json          # Settings (PermitLimit, QueueLimit, Timeout)
│── main.go              # Gin setup
│── README.md

⚙️ Config Example (config.json)
{
  "requests_per_second": 5,
  "queue_limit": 10,
  "timeout": 2
}

🧩 Key Concept — struct{}{}

In Go, struct{}{} is used here as a token in channels:

struct{} → type (an empty struct type).

struct{}{} → value of that type (an actual empty struct).

We use it because:

It takes zero bytes (memory efficient).

We don’t need to store data, just mark a slot as taken/free.

Example:

sem := make(chan struct{}, 5) // max 5 concurrent
sem <- struct{}{}             // take a slot
<-sem                         // release slot

🚀 Run the Project
go run main.go


✅ Behavior
Only PermitLimit requests run at once.
Extra requests go into QueueLimit.
If queue is full → return 503.
If waiting too long → return 429.
