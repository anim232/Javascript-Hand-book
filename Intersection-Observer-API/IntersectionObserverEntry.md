### **`IntersectionObserverEntry` এর ব্যবহার**

`IntersectionObserverEntry` ইন্টারফেসটি `IntersectionObserver API` এর একটি অংশ এবং এটি লক্ষ্য এলিমেন্টের এবং তার রুট কন্টেইনারের মধ্যে ইন্টারসেকশনের (অথবা সংযোগের) পরিস্থিতি বর্ণনা করে। এই তথ্যগুলোকে ব্যবহার করে আমরা জানাতে পারি কখন কোনো এলিমেন্ট দৃশ্যমান হচ্ছে বা গায়েব হচ্ছে, এবং এর মানে কীভাবে এলিমেন্টটি স্ক্রিনে বা স্ক্রল ভিউপোর্টে প্রবেশ করছে।

এখানে, আমি আরও কিছু বাস্তব উদাহরণ দেব এবং কোডের ধাপে ধাপে ব্যাখ্যা করব কিভাবে `IntersectionObserverEntry` কাজ করে।

---

### **এলিমেন্টের ভিউপোর্টে দৃশ্যমানতা ট্র্যাক করা**

আমরা একটি `IntersectionObserver` তৈরি করব যা একটি এলিমেন্টের দৃশ্যমানতা ট্র্যাক করবে এবং ভিউপোর্টে এলিমেন্টটির দৃশ্যমানতার পরিবর্তন দেখাবে।

#### **HTML কোড**:
```html
<div id="target" style="width: 200px; height: 200px; background-color: red; margin-top: 500px;">
  Target Element
</div>

<script src="script.js"></script>
```

#### **CSS কোড**:
```css
body {
  height: 2000px; /* পেজে স্ক্রল করার জন্য পেইজটি বড় করা */
}
```

#### **JavaScript কোড**:
```javascript
// Intersection Observer তৈরি করা
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log("Element is visible in the viewport.");
      console.log("Intersection Ratio: " + entry.intersectionRatio);
      console.log("Target Element: ", entry.target);
      console.log("Root bounds: ", entry.rootBounds);
      console.log("Bounding Client Rect: ", entry.boundingClientRect);
    } else {
      console.log("Element is not visible in the viewport.");
    }
  });
}, {
  root: null,  // ভিউপোর্ট (ডিফল্ট)
  rootMargin: "0px",  // কোন মার্জিন থাকবে না
  threshold: 0.5  // ৫০% দৃশ্যমান হলে ট্রিগার হবে
});

// লক্ষ্য এলিমেন্টটি সিলেক্ট করা এবং অবজার্ভ করা
const targetElement = document.getElementById("target");
observer.observe(targetElement);
```

---

### **ধাপে ধাপে কোডের ব্যাখ্যা**:

1. **`IntersectionObserver` তৈরি করা**:
   - প্রথমে একটি `IntersectionObserver` ইনস্ট্যান্স তৈরি করা হয়েছে। এই অবজার্ভারটি যখন কোনো এলিমেন্ট ভিউপোর্টে প্রবেশ করবে, তখন কলব্যাক ফাংশনটি ট্রিগার হবে।
   - কলব্যাক ফাংশনে আমরা `entries` প্যারামিটারটি পাচ্ছি যা `IntersectionObserverEntry` এর একটি অ্যারে।

2. **Callback function**:
   - `entries.forEach(entry => {...})`: এই অংশে প্রতিটি `entry` একটি `IntersectionObserverEntry` ইনস্ট্যান্স যা আমাদের দেওয়া হয়েছে। এটি এলিমেন্টের অবস্থান এবং অন্যান্য ইন্টারসেকশন সম্পর্কিত ডেটা ধারণ করে।
   
3. **`entry.isIntersecting`**:
   - `isIntersecting` প্রপার্টি ব্যবহার করে আমরা চেক করছি যে, `entry.target` এলিমেন্টটি বর্তমানে ভিউপোর্টের সাথে ইন্টারসেক্ট করছে কি না।
   - যদি এটি `true` হয়, তা হলে এলিমেন্টটি দৃশ্যমান (অথবা ভিউপোর্টে এসেছে)।
   - যদি এটি `false` হয়, এলিমেন্টটি ভিউপোর্টের বাইরে চলে গেছে।

4. **`entry.intersectionRatio`**:
   - `intersectionRatio` হল একটি মান (০ থেকে ১ পর্যন্ত) যা বলে দেয় এলিমেন্টের কতটুকু অংশ দৃশ্যমান। উদাহরণস্বরূপ, যদি এলিমেন্টের ৫০% অংশ দৃশ্যমান থাকে, তবে এটি ০.৫ হবে।

5. **`entry.target`**:
   - `entry.target` হল সেই `Element` যেটির ইন্টারসেকশন পরিবর্তিত হয়েছে। এখানে এটি `target` এলিমেন্ট, অর্থাৎ সেই এলিমেন্ট যেটি আমরা অবজার্ভ করছি।

6. **`entry.rootBounds`**:
   - `rootBounds` হল রুট কন্টেইনারের বাউন্ডিং রেকট্যাঙ্গেল (যেমন ভিউপোর্ট)। এটি একটি `DOMRectReadOnly` অবজেক্ট যা জানায় যে রুট এলিমেন্টটি কতটুকু বড় এবং কোথায় অবস্থিত।

7. **`entry.boundingClientRect`**:
   - `boundingClientRect` হল লক্ষ্য এলিমেন্টের বাউন্ডিং রেকট্যাঙ্গেল, অর্থাৎ এলিমেন্টটির আকার এবং অবস্থান ভিউপোর্টের প্রতি।

---

### **ফলাফল**:
আপনি যখন পেজটি স্ক্রল করবেন, তখন যদি `#target` এলিমেন্টটি ভিউপোর্টে প্রবেশ করে, তখন কনসোলে এই ধরনের বার্তা দেখা যাবে:
```
Element is visible in the viewport.
Intersection Ratio: 0.5
Target Element: <div id="target" style="width: 200px; height: 200px; background-color: red; margin-top: 500px;">
Root bounds: DOMRectReadOnly {...}
Bounding Client Rect: DOMRectReadOnly {...}
```

এটি জানাচ্ছে যে `#target` এলিমেন্টটি ৫০% দৃশ্যমান (যেহেতু `threshold: 0.5` ব্যবহার করা হয়েছে), এবং এর অবস্থান এবং আকার কনসোলে প্রদর্শিত হচ্ছে।

---

### **আরও উদাহরণ: এলিমেন্টের স্কেল এবং রং পরিবর্তন করা**

#### **লক্ষ্য**:
যখন কোনো এলিমেন্ট স্ক্রল করে দৃশ্যমান হবে, তখন তার আকার পরিবর্তন করতে এবং রং পরিবর্তন করতে হবে।

#### **HTML কোড**:
```html
<div id="target" style="width: 200px; height: 200px; background-color: red; margin-top: 500px;">
  Target Element
</div>

<script src="script.js"></script>
```

#### **JavaScript কোড**:
```javascript
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.style.transform = "scale(1.5)";
      entry.target.style.backgroundColor = "green";
    } else {
      entry.target.style.transform = "scale(1)";
      entry.target.style.backgroundColor = "red";
    }
  });
}, {
  root: null,
  rootMargin: "0px",
  threshold: 0.5
});

const targetElement = document.getElementById("target");
observer.observe(targetElement);
```

#### **ধাপে ধাপে ব্যাখ্যা**:
1. **Observer তৈরি করা**: আগের মতোই একটি `IntersectionObserver` তৈরি করা হয়েছে, যেখানে `entries` থেকে প্রত্যেকটি `IntersectionObserverEntry` ইনস্ট্যান্স এর ডেটা প্রসেস করা হচ্ছে।
2. **`isIntersecting` চেক করা**: যদি `entry.isIntersecting` `true` হয়, তখন এলিমেন্টটির আকার ১.৫ গুণ বড় করা হবে এবং এর ব্যাকগ্রাউন্ড রং সবুজ করা হবে।
3. **`scale` এবং `backgroundColor` পরিবর্তন**: `scale(1.5)` এবং `backgroundColor` প্রপার্টি পরিবর্তন করা হচ্ছে, যাতে এলিমেন্টের দৃশ্যমানতা অনুযায়ী তার আকার এবং রং পরিবর্তিত হয়।


এখন, যখন আপনি স্ক্রল করবেন এবং `target` এলিমেন্টটি ৫০% দৃশ্যমান হবে, তখন তার আকার বড় হয়ে যাবে এবং রঙ সবুজ হয়ে যাবে। যদি এলিমেন্টটি দৃশ্যমান না থাকে, তবে আকার স্বাভাবিক অবস্থায় ফিরে যাবে এবং রং আবার লাল হয়ে যাবে।

---


`IntersectionObserverEntry` হল `IntersectionObserver` API এর একটি গুরুত্বপূর্ণ অংশ যা আপনাকে একটি এলিমেন্টের ভিউপোর্টের প্রতি ইন্টারসেকশন সম্পর্কিত ডেটা প্রদান করে। এর বিভিন্ন প্রপার্টি যেমন `intersectionRatio`, `boundingClientRect`, `isIntersecting` ইত্যাদি ব্যবহার করে আপনি আপনার ওয়েবপেজের এলিমেন্টের দৃশ্যমানতা ট্র্যাক এবং নিয়ন্ত্রণ করতে পারেন।
