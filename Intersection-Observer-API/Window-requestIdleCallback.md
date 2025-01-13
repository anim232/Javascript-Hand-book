### **Window.requestIdleCallback() এর ব্যবহার:**

`Window.requestIdleCallback()` একটি মেথড যা ওয়েব ব্রাউজারকে পরামর্শ দেয় যখন পেজে প্রক্রিয়া করার জন্য পর্যাপ্ত ফ্রি সময় থাকে তখন কিছু কাজ সম্পন্ন করার জন্য। এটি আপনার স্ক্রিপ্টকে হালকা কাজ বা ব্যাকগ্রাউন্ড কাজ চালানোর জন্য আদর্শ, যেগুলি পেজ লোড হওয়ার সময়ের উপর প্রভাব ফেলে না এবং ব্যবহারকারীর ইন্টারঅ্যাকশনে কোনো বিঘ্ন সৃষ্টি করে না।

এটি এমন পরিস্থিতিতে সাহায্য করতে পারে যখন আপনার ওয়েব পেজের কাজ করার জন্য কিছু অতিরিক্ত সময় দরকার, কিন্তু আপনি চাইবেন না যে কাজগুলো পেজের ইন্টারঅ্যাকশনের সময় বিরক্তিকর হয়ে উঠুক।

### **ধাপে ধাপে ব্যাখ্যা:**

`requestIdleCallback` মেথডটি তখনই কল করা যায় যখন ব্রাউজারের থ্রেড ফ্রি থাকে (অর্থাৎ ব্রাউজার কোনো গুরুতর কাজ করছে না) এবং এটি অ্যাসিঙ্ক্রোনাসভাবে কাজ করে। এই মেথডটি একটি কলব্যাক ফাংশন গ্রহণ করে, যা তখন চালানো হয় যখন ব্রাউজারের "অপেক্ষমান" অবস্থা থাকে।


### **সাধারণ সিনট্যাক্স:**

```javascript
window.requestIdleCallback(callback, options);
```

- **callback**: এটি একটি ফাংশন যা কল করা হবে যখন ব্রাউজারের থ্রেড ফ্রি হবে।
- **options**: একটি অবজেক্ট যা অতিরিক্ত অপশন নির্দিষ্ট করতে পারে। আপনি `timeout` প্রপার্টি ব্যবহার করতে পারেন, যা মেথডটিকে একটি নির্দিষ্ট সময়ের জন্য কল করার জন্য ডিফাইন করবে।

---

### **উদাহরণ ১: একটি সিম্পল `requestIdleCallback` ব্যবহার করে কাজ করা**

#### **HTML কোড**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>requestIdleCallback Example</title>
</head>
<body>
  <h1>Welcome to the Idle Callback Example</h1>
  <script src="script.js"></script>
</body>
</html>
```

#### **JavaScript কোড**:
```javascript
function doSomeBackgroundTask(deadline) {
  while (deadline.timeRemaining() > 0 && tasks.length > 0) {
    const task = tasks.shift();
    task();
  }

  if (tasks.length > 0) {
    // যদি কাজের তালিকা শেষ না হয়, তাহলে পুনরায় requestIdleCallback ব্যবহার করা হবে
    window.requestIdleCallback(doSomeBackgroundTask);
  }
}

// কিছু ব্যাকগ্রাউন্ড কাজের জন্য একটি তালিকা তৈরি করা
const tasks = [
  () => console.log('Task 1 completed'),
  () => console.log('Task 2 completed'),
  () => console.log('Task 3 completed')
];

// idle callback শুরু করা
window.requestIdleCallback(doSomeBackgroundTask);
```

#### **ধাপে ধাপে ব্যাখ্যা**:

1. **`doSomeBackgroundTask(deadline)` ফাংশন**:
   - এখানে `doSomeBackgroundTask` ফাংশনটি মূল ফাংশন যা ব্যাকগ্রাউন্ডে কাজ করে। 
   - `deadline` প্যারামিটারটি `requestIdleCallback` এর মাধ্যমে প্রাপ্ত একটি অবজেক্ট যা জানায় কতটুকু সময় আমাদের কাজে ব্যয় করার জন্য অবশিষ্ট আছে (`timeRemaining()` ব্যবহার করে)।

2. **`tasks` অ্যারে**:
   - এটি একটি সিম্পল অ্যারে যা কিছু ব্যাকগ্রাউন্ড টাস্ক ধারণ করে। এখানে শুধু `console.log` দিয়ে কাজগুলি করা হয়েছে।

3. **`timeRemaining()`**:
   - `deadline.timeRemaining()` মেথডটি বলে দেয় কতটুকু সময় এখনও বাকি রয়েছে, সেই সময়টা ব্যবহার করে `doSomeBackgroundTask` ফাংশনটি কাজ সম্পন্ন করতে পারে।

4. **`window.requestIdleCallback(doSomeBackgroundTask)`**:
   - যখন ব্রাউজারের থ্রেড ফ্রি থাকবে, তখন `requestIdleCallback` মেথডটি `doSomeBackgroundTask` ফাংশনটিকে কল করবে এবং যেটি ব্যাকগ্রাউন্ড টাস্কগুলি সম্পন্ন করবে।

5. **ব্যাকগ্রাউন্ড কাজের তালিকা**:
   - যখন একটি কাজ শেষ হয়ে যায়, তাহলে পরবর্তী কাজটি নিয়ে কাজ শুরু হবে। যদি কোনো কাজ শেষ না হয়, তবে `requestIdleCallback` আবার কল হয়ে থাকবে যতক্ষণ না সমস্ত কাজ সম্পন্ন না হয়।

---

### **উদাহরণ ২: টাইমআউট দিয়ে `requestIdleCallback` ব্যবহার করা**

এখন, একটি উদাহরণ দেখব যেখানে `timeout` অপশন ব্যবহার করে আমরা টাইমআউট নির্দিষ্ট করব। যদি ব্রাউজারের থ্রেড দীর্ঘ সময় ধরে ফ্রি না থাকে, তবে `requestIdleCallback` কার্যকর হবে না এবং আমাদের ফাংশনটি আর এক্সিকিউট হবে না।

#### **JavaScript কোড**:
```javascript
function performTask(deadline) {
  console.log('Performing background task...');

  // যদি কোনো কাজ না হয়, তবে ব্রাউজারকে পুনরায় idle callback করতে বলা হবে
  if (deadline.timeRemaining() > 0) {
    console.log('Task completed!');
  } else {
    console.log('Task is taking too long...');
  }
}

// টাইমআউটের সাথে idle callback কল করা
window.requestIdleCallback(performTask, { timeout: 2000 });
```

#### **ধাপে ধাপে ব্যাখ্যা**:

1. **`performTask(deadline)` ফাংশন**:
   - `performTask` ফাংশনটি ব্যাকগ্রাউন্ডে কাজ করবে। যদি ব্রাউজারের থ্রেড ফ্রি থাকে এবং প্রয়োজনীয় সময় থাকে, এটি কাজটি সম্পন্ন করবে। অন্যথায়, এটি জানিয়ে দেবে যে কাজটি বেশি সময় নিয়ে ফেলেছে।
   
2. **`timeout` অপশন**:
   - `timeout: 2000` দ্বারা আমরা ২০০০ মিলিসেকেন্ড (২ সেকেন্ড) পর কাজটি শুরু করতে চাই। এর মানে হলো, যদি ব্রাউজার ২ সেকেন্ডের মধ্যে ফ্রি না হয়, তাহলে `requestIdleCallback` কল করা হবে না এবং কাজটি করা হবে না।

---

### **উদাহরণ ৩: অ্যাসিনক্রোনাস কাজ এবং স্টেট আপডেট**

আমরা অ্যাসিনক্রোনাস কাজের জন্য `requestIdleCallback` ব্যবহার করব, যেখানে স্ক্রিপ্টের কিছু সময় ব্যাকগ্রাউন্ডে চলবে এবং তারপর স্টেট আপডেট করা হবে।

#### **HTML কোড**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Request Idle Callback Example</title>
</head>
<body>
  <h1>Background Task Example</h1>
  <div id="status">Status: Idle</div>
  <script src="script.js"></script>
</body>
</html>
```

#### **JavaScript কোড**:
```javascript
let statusDiv = document.getElementById("status");

function updateStatus(deadline) {
  if (deadline.timeRemaining() > 0) {
    statusDiv.textContent = "Status: Working...";
  } else {
    statusDiv.textContent = "Status: Waiting for idle time...";
  }
}

function doBackgroundWork() {
  // কিছু ব্যাকগ্রাউন্ড কাজের পর স্টেট আপডেট করা
  window.requestIdleCallback(updateStatus);
}

doBackgroundWork();
```

#### **ধাপে ধাপে ব্যাখ্যা**:

1. **`updateStatus(deadline)`**:
   - এই ফাংশনটি স্টেট আপডেট করবে, যদি কিছু সময় অবশিষ্ট থাকে তবে "Working..." স্টেট প্রদর্শন করবে, আর না থাকলে "Waiting for idle time..." প্রদর্শন করবে।

2. **`doBackgroundWork()`**:
   - এই ফাংশনটি ব্যাকগ্রাউন্ড কাজ করার জন্য `requestIdleCallback` ব্যবহার করবে, যা যখন ব্রাউজারের থ্রেড ফ্রি থাকবে তখন `updateStatus` কল করবে।

3. **স্টেট আপডেট**:
   - পেজের `status` এলিমেন্টে "Working..." বা "Waiting for idle time..." টেক্সট দেখানো হবে, যা ব্রাউজারের অবস্থা অনুযায়ী পরিবর্তিত হবে।

---



`requestIdleCallback()` এমন কাজ করার জন্য একটি শক্তিশালী টুল, যা ব্যবহারকারী ইন্টারঅ্যাকশনের উপর কোনো প্রভাব না ফেলেই ব্যাকগ্রাউন্ডে কাজ করতে দেয়। এই মেথডটি তখনি কার্যকর হয় যখন ব্রাউজারের থ্রেড ফ্রি থাকে এবং আপনি কিছু হালকা কাজ করতে চান। তবে মনে রাখবেন, এটি সব ব্রাউজারে সমর্থিত নয়, তাই এর ব্যবহার করার আগে ব্রাউজারের সমর্থন চেক করা উচিত।
