# Môi Trường Kỹ Thuật: Ứng Dụng Quản Lý Công Việc (Todo List)

## Ngôn Ngữ và Trình Quản Lý Gói

- **Node.js 22** (LTS)
- **npm** để quản lý các gói (packages)
- `package.json` để cấu hình dự án và các tập lệnh (scripts)

## Framework Backend

- **Express.js** cho máy chủ API REST
- Lưu trữ dữ liệu trên bộ nhớ (sử dụng Map/Array thuần của JavaScript) - không yêu cầu cơ sở dữ liệu cho MVP
- Gói **uuid** để tạo ID cho các công việc

## Framework Frontend

- **React 19** với các function component và hooks
- **Vite** làm công cụ xây dựng (build tool) và máy chủ phát triển (dev server)
- CSS thuần (không yêu cầu framework CSS)

## Cấu Trúc Dự Án

```
todo-app/
├── package.json
├── server/
│   ├── index.js          # Điểm đầu vào của máy chủ Express
│   ├── routes/
│   │   └── todos.js      # Các route CRUD cho công việc (Todo)
│   └── store.js          # Lưu trữ công việc trên bộ nhớ
├── client/
│   ├── index.html
│   ├── src/
│   │   ├── main.jsx      # Điểm đầu vào của React
│   │   ├── App.jsx       # Component gốc
│   │   ├── components/
│   │   │   ├── TodoInput.jsx
│   │   │   ├── TodoList.jsx
│   │   │   ├── TodoItem.jsx
│   │   │   └── FilterBar.jsx
│   │   ├── hooks/
│   │   │   └── useTodos.js
│   │   └── styles/
│   │       └── App.css
│   └── vite.config.js
└── tests/
    ├── server/
    │   └── todos.test.js  # Các bài kiểm thử tích hợp API
    └── client/
        └── App.test.jsx   # Các bài kiểm thử component
```

## Kiểm Thử (Testing)

- **Vitest** cho cả kiểm thử máy chủ (server) và máy khách (client)
- **React Testing Library** cho kiểm thử component
- **supertest** cho kiểm thử tích hợp API
- Các bài kiểm thử được chạy thông qua lệnh `npm test`

## Các Lệnh Phát Triển (Development Scripts)

```json
{
  "scripts": {
    "dev": "concurrently \"npm run dev:server\" \"npm run dev:client\"",
    "dev:server": "node server/index.js",
    "dev:client": "vite client",
    "build": "vite build client",
    "test": "vitest run",
    "start": "node server/index.js"
  }
}
```

## Quy Ước (Conventions)

- ES modules (`"type": "module"` trong package.json)
- Máy chủ lắng nghe trên cổng 3001, được proxy từ Vite dev server trên cổng 5173
- Tất cả các route API đều có tiền tố `/api/`
- Endpoint kiểm tra tình trạng (health endpoint) tại `/health` (không có tiền tố `/api/`)
- Mật mã trạng thái HTTP tiêu chuẩn: 200, 201, 400, 404, 500
