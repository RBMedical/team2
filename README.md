# Check Up Flow System

ระบบลงทะเบียน Mobile Check Up — แปลงจาก Vanilla HTML/CSS/JS เป็น **Next.js 14 + TypeScript + Tailwind CSS + shadcn/ui**

## โครงสร้างโปรเจ็กต์

```
checkup-flow/
├── app/
│   ├── globals.css          ← Tailwind + CSS Variables + original styles ported
│   ├── layout.tsx           ← Root layout (Sarabun font, JsBarcode CDN, Toaster)
│   └── page.tsx             ← Main page (Sidebar + Pages)
├── components/
│   ├── ui/
│   │   ├── alert-dialog.tsx ← shadcn AlertDialog ✅ (component ที่ขอ)
│   │   ├── button.tsx       ← shadcn Button (required by alert-dialog)
│   │   ├── toast.tsx        ← shadcn Toast
│   │   └── toaster.tsx      ← Toast renderer
│   ├── ConfirmDialog.tsx    ← Wrapper ใช้ AlertDialog แทน SweetAlert2
│   ├── AddNewModal.tsx      ← Modal เพิ่มรายชื่อใหม่
│   ├── EditModal.tsx        ← Modal แก้ไขข้อมูล
│   ├── RegistrationPage.tsx ← หน้าลงทะเบียนหลัก (แปลงจาก app.js ครบ)
│   ├── ReportPage.tsx       ← หน้ารายงาน/ติดตาม
│   └── SpecimenModal.tsx    ← Modal นับสิ่งตรวจ
├── hooks/
│   └── use-toast.ts         ← Toast state hook
├── lib/
│   ├── api.ts               ← Google Apps Script JSONP client (TypeScript)
│   └── utils.ts             ← cn() utility (required by shadcn)
├── types/
│   └── index.ts             ← TypeScript types ทั้งหมด
├── components.json          ← shadcn config
├── tailwind.config.ts       ← Tailwind config + shadcn preset
├── tsconfig.json            ← TypeScript config + path alias @/*
└── package.json
```

## การติดตั้ง

```bash
# 1. ติดตั้ง dependencies
npm install

# 2. รัน development server
npm run dev
```

เปิดเบราว์เซอร์ที่ http://localhost:3000

## การเพิ่ม shadcn components อื่น ๆ

```bash
# ตัวอย่างเพิ่ม component อื่น
npx shadcn@latest add dialog
npx shadcn@latest add select
npx shadcn@latest add input
```

## การใช้ AlertDialog (component ที่ขอ)

```tsx
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogTrigger,
} from "@/components/ui/alert-dialog"

// ใช้ผ่าน ConfirmDialog wrapper (แนะนำ)
import { ConfirmDialog } from "@/components/ConfirmDialog"

<ConfirmDialog
  open={open}
  title="ยืนยันการลบ"
  description="การกระทำนี้ไม่สามารถย้อนกลับได้"
  confirmText="ลบ"
  cancelText="ยกเลิก"
  variant="destructive"
  onConfirm={handleDelete}
  onCancel={() => setOpen(false)}
/>
```

## สิ่งที่เปลี่ยนจาก Vanilla → React/TypeScript

| เดิม | ใหม่ |
|---|---|
| `Swal.fire()` confirm dialogs | `AlertDialog` (shadcn) ผ่าน `ConfirmDialog` component |
| `Swal.fire()` toast/success | shadcn `Toast` / `useToast()` hook |
| `document.getElementById()` | React state + controlled inputs |
| Bootstrap 5 CDN | Tailwind CSS |
| Lucide UMD CDN | `lucide-react` package |
| Vanilla JS JSONP | TypeScript `appScriptRequest<T>()` ใน `lib/api.ts` |
| ไฟล์เดียว app.js | แยก components, hooks, types, lib |

## ตั้งค่า Google Apps Script URL

แก้ URL ใน `lib/api.ts`:

```ts
const APP_SCRIPT_URL = "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec";
```
