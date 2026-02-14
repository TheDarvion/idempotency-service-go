# Idempotency Service (Go)

![Status](https://img.shields.io/badge/status-In%20Progress-orange)  
![Language](https://img.shields.io/badge/language-Go-blue)

---
## Проект в активной разработке, план завершения первой версии: весна 2026
---

## Описание
Idempotency Service — это middleware для HTTP API на Go, который обеспечивает безопасную обработку повторных запросов к критическим операциям (например, платежи).  
Цель проекта — изучить работу с инфраструктурными сервисами, хранение состояния и безопасное управление повторными запросами.

---

## Функциональность
- Обработка повторных запросов с использованием `Idempotency-Key`  
- Хранение результата выполнения запроса на определённое время (TTL)  
- Повторный запрос с тем же ключом возвращает предыдущий результат без повторного выполнения операции  
- Использование PostgreSQL для хранения ключей  
- Unit-тесты для проверки корректной работы  

---

## План развития
1. Создать базовый middleware для обработки Idempotency-Key  
2. Добавить хранение ключей и результатов запросов  
3. Интегрировать PostgreSQL для долговременного хранения  
4. Добавить unit-тесты  
5. В будущем: оптимизация TTL, поддержка разных стратегий для ключей  

---

## Стек технологий
- **Язык:** Go  
- **Сервер:** net/http (или chi/mux)  
- **Хранилище:** PostgreSQL  
- **Тестирование:** Go testing package  

---

## Пример использования (планируемый)
```go
package main

import (
    "net/http"
    "github.com/TheDarvion/idempotency-service-go/service"
)

func main() {
    isvc := service.NewIdempotencyService()

    http.Handle("/payment", isvc.Middleware(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Payment processed"))
    })))

    http.ListenAndServe(":8080", nil)
}
