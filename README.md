# TypeScript Deep Dive: Interface vs Type, any, unknown, never (Bangla Version — Conceptual + Practical)

TypeScript শুধু JavaScript-কে টাইপ দেওয়ার একটি ভাষা নয়—এটি একটি **চিন্তাভাবনার পদ্ধতি**। এটি প্রজেক্টকে নিরাপদ, পূর্বানুমানযোগ্য এবং maintainable করে তোলে। তাই TypeScript শিখতে হলে এর টাইপ সিস্টেম শুধু ব্যবহারিক দিক থেকে নয়—**দর্শনগতভাবে** বুঝে নেওয়া জরুরি।

এই README-তে আমরা TypeScript-এর ৫টি মূল ধারণাকে **ব্যবহারিক + কনসেপ্টচুয়াল** দৃষ্টিভঙ্গিতে ব্যাখ্যা করবো:

✔ Interface
✔ Type Alias
✔ any
✔ unknown
✔ never

---

## 🚀 পার্ট ১: Interface এবং Type — শুধু ব্যবহার নয়, তাদের দর্শনও বুঝি

Interface এবং Type Alias—দুটোই structure define করে, কিন্তু তাদের দর্শন, উদ্দেশ্য ও ব্যবহারিক কৌশল আলাদা।

---

## 🎯 Interface: "Contract of Structure"

Interface হলো একটি **চুক্তি (contract)**—যা বলে দেয় একটি structure-এ কোন কোন field থাকবে।

Interface-এর মূল দর্শন:

> **“Shape of data should be predictable and extendable.”**

এটি ব্যবহৃত হয় যখন:

- কোন সিস্টেমের domain model তৈরি করা হয়
- class-based architecture থাকে
- structure নিয়মিতভাবে expand করার প্রয়োজন হবে

```ts
interface User {
  id: number;
  name: string;
  email: string;
}
```

Extend করা সহজ:

```ts
interface Admin extends User {
  role: "super-admin" | "editor";
}
```

Interface ব্যবহৃত হয় যখন **structure-first design** দরকার।

---

## 🎯 Type Alias: "Label + Powerful Type Composition"

Type Alias নতুন টাইপ তৈরি করে না—বরং বিদ্যমান টাইপকে একটি নাম দেয়। কিন্তু এর প্রকৃত শক্তি **composition এবং flexibility**।

Type Alias-এর দর্শন:

> **“Types can be composed, transformed, or calculated.”**

Type Alias দিয়ে করা যায়:

- Union
- Intersection
- Conditional Types
- Mapped Types
- Utility Types

```ts
type Status = "active" | "inactive" | "pending";

type ApiResponse<T> = {
  data: T;
  success: boolean;
  error?: string;
};
```

Type Alias ব্যবহারের মূল শক্তি: **Logical + Composable typing**।

---

## 🔍 Interface vs Type — ধারণাগত তুলনা

| প্রশ্ন          | Interface                     | Type Alias                    |
| --------------- | ----------------------------- | ----------------------------- |
| জন্মের উদ্দেশ্য | Object design + expandability | Complex type composition      |
| স্কেলিং ক্ষমতা  | Structural scaling শক্তিশালী  | Logical composition শক্তিশালী |
| Readability     | অনেক পরিষ্কার                 | বেশি expressive               |
| Utility typing  | কম                            | খুব শক্তিশালী                 |

---

## ⚠️ any: TypeScript বন্ধ করার সুইচ

`any` শুধু একটি টাইপ নয়—it disables TypeScript.

TypeScript-এর উদ্দেশ্য:

> **Error আগে থেকেই ধরতে সাহায্য করা।**

`any` বলে:

> “Type checking-এর দরকার নেই—সব ঠিক আছে।”

এটি debugging, refactoring, और scaling—সবকিছু বিপজ্জনক করে তোলে।

```ts
async function sendEmail(emailData: any) {
  const recipient = emailData.to.toLowerCase();
}
```

যদি `to` array হয়?
❌ অ্যাপ্লিকেশন ক্র্যাশ করবে
❌ Debug করা কঠিন
❌ কোন warning নেই

`any` ব্যবহার মানে TypeScript-কে JavaScript-এ **downgrade** করা।

---

## 🛡️ unknown: Responsible Freedom

`unknown` হলো type-safe programming mindset গড়ার টুল।

এর দর্শন:

> **“You are free, but you must validate.”**

```ts
const data: unknown = await response.json();

if (!isValidUser(data)) {
  throw new Error("Invalid user data");
}

return data;
```

Benefits:

- ডেটা validate করতে বাধ্য করা
- external API থেকে সুরক্ষা
- predictable & safe কোড

unknown = **Trust but verify philosophy**।

---

# 🔥 পার্ট ২: never, Interface vs Type — গভীর ব্যাখ্যা

---

## 🧩 never: Impossible State Detector

`never` শুধু টাইপ নয়—এটা একটি সিগন্যাল।

এর উদ্দেশ্য:

> **“এই অবস্থায় কখনো পৌঁছানোর কথা নয়।”**

এটি exhaustive checking enforce করতে অসাধারণ।

```ts
function getEmailStatusLabel(status: "draft" | "sent" | "failed") {
  switch (status) {
    case "draft":
      return "📝 Draft";
    case "sent":
      return "✅ Sent";
    case "failed":
      return "❌ Failed";
    default:
      const exhaustive: never = status;
      return exhaustive;
  }
}
```

নতুন status যোগ করলে compiler warning দেবে → future-proof logic।

---

## 🏛 Conceptual Summary: Interface vs Type

### Interface

- Structural
- Expandable
- Class-friendly
- Team-friendly
- Model/Schema design-এ natural fit

### Type Alias

- Logical
- Composable
- Union & intersection heavy logic
- Advanced type transformation
- Utility-based design-friendly

---

## 🧭 Practical + Conceptual পরামর্শ

### ✔ Interface ব্যবহার করুন যখন:

- Domain model ডিজাইন করছেন
- API schema লিখছেন
- Class implement করতে হবে
- Structure scalable হতে হবে

### ✔ Type Alias ব্যবহার করুন যখন:

- Union/Intersection দরকার
- Advanced logic-based typing করছেন
- Conditional বা mapped types বানাচ্ছেন
- Reusable generic types লিখছেন

### ✔ unknown ব্যবহার করুন যখন:

- API response unpredictable
- User input validate করতে হবে
- Defensive programming দরকার

### ✔ never ব্যবহার করুন যখন:

- Switch-case exhaustive করতে চান
- Impossible state ধরতে চান
- Future-proof logic চান

---

## ✅ উপসংহার — TypeScript মানে ভালো সফটওয়্যার চিন্তাধারা

TypeScript-এর মর্মবস্তু হলো **predictability + safety**।

- `any` shortcut দেয়, কিন্তু dangerous
- `unknown` responsibility শেখায়
- `never` discipline enforce করে
- Interface structure শেখায়
- Type Alias logic শেখায়

TypeScript শেখা মানে শুধু syntax নয়—**better engineering principles শেখা**।

💙 শুভ TypeScript কোডিং!
Smart, safe, predictable applications তৈরি করতে থাকুন।
