**Intersection Observer API**

**Intersection Observer API** একটি পদ্ধতি প্রদান করে যা সাহায্যে আমরা অ্যাসিঙ্ক্রোনাসভাবে লক্ষ্য উপাদানের সাথে তার পূর্বসূরি উপাদান বা শীর্ষ-স্তরের ডকুমেন্ট ভিউপোর্টের সাথে ইন্টারসেকশনের (অবস্থান বা সীমানা মিলানো) পরিবর্তনগুলি পর্যবেক্ষণ করতে পারি।

এটি সম্ভবত একটি পুরানো সমস্যা যা ওয়েব পৃষ্ঠায় উপাদানের দৃশ্যমানতা বা দুটি উপাদানের একে অপরের সাথে সম্পর্কিত দৃশ্যমানতা সনাক্ত করতে চেয়েছে। পূর্বে এ ধরনের সমস্যা সমাধান করার উপায়গুলো অপ্রত্যাশিত এবং অপ্রতুল ছিল, যা ব্রাউজার এবং সাইটের পারফরম্যান্সকে ধীর করে দিতে পারত। তবে, ওয়েবের বিকাশের সঙ্গে সঙ্গে এ ধরনের তথ্যের প্রয়োজনীয়তা বৃদ্ধি পায়।

**কিছু গুরুত্বপূর্ণ কারণ, যেখানে Intersection Observer API প্রয়োজনীয়:**
1. **Lazy-loading**: যখন একটি পেজ স্ক্রোল করা হয়, তখন ছবিগুলি বা অন্যান্য কন্টেন্ট লোড করা।
2. **Infinite scrolling**: ওয়েবসাইটে কনটেন্ট লোড করার জন্য এটি ব্যবহার করা হয়, যেখানে স্ক্রল করার সাথে সাথে আরও কনটেন্ট লোড হয়।
3. **অ্যাডভাটাইজমেন্ট ভিজিবিলিটি ট্র্যাক করা**: বিজ্ঞাপনের দৃশ্যমানতা পরিমাপ করে আয়ের হিসাব করা।
4. **এনিমেশন বা কার্যক্রম পরিচালনা**: যদি ব্যবহারকারী কন্টেন্টটি দেখতে পারেন, তবে তার ভিত্তিতে এনিমেশন বা অন্য কার্যক্রম পরিচালনা করা।

**পুরানো পদ্ধতিতে সমস্যা:**
পুরানো পদ্ধতিতে, একাধিক লাইব্রেরি ব্যবহার করে বিভিন্ন উপাদানকে পর্যবেক্ষণ করতে হতো, এবং এই পর্যবেক্ষণ কোডগুলো মূল থ্রেডে চলত। এতে করে পারফরম্যান্স সমস্যা হতে পারে, বিশেষত যখন অনেক এলিমেন্টের জন্য এই কাজ করা হয়।

**Intersection Observer API কিভাবে কাজ করে:**
এই API-র মাধ্যমে, ডেভেলপাররা একটি **callback function** রেজিস্টার করতে পারেন, যা কোনও নির্দিষ্ট উপাদান অন্য একটি উপাদানের বা ভিউপোর্টের সাথে ইন্টারসেক্ট করলে বা তাদের ইন্টারসেকশন একটি নির্দিষ্ট পরিমাণে পরিবর্তিত হলে চালিত হবে। এর ফলে, সাইটগুলো আর মূল থ্রেডে এই পরীক্ষা চালাতে হবে না, এবং ব্রাউজার নিজেই পারফরম্যান্স অপটিমাইজ করতে পারবে।

**কিছু সীমাবদ্ধতা:**
এই API-র মাধ্যমে, আপনি নির্দিষ্টভাবে কতটা পিক্সেল ওভারল্যাপ হয়েছে, তা জানার সুযোগ নেই। এটি সাধারণভাবে ব্যবহার করা হয়, যেমন: "যদি তারা N% পরিমাণে ইন্টারসেক্ট করে, তবে আমি কিছু করতে চাই।"

এইভাবে, Intersection Observer API ওয়েব পৃষ্ঠায় পিজল বা কনটেন্টের দৃশ্যমানতা ট্র্যাক করতে একটি দক্ষ এবং উন্নত পদ্ধতি প্রদান করে।


**Intersection Observer API: ধারণা এবং ব্যবহার**

**Intersection Observer API** আপনাকে একটি callback কনফিগার করার সুযোগ দেয়, যা যখন নির্দিষ্ট কিছু ঘটনা ঘটে তখন কল হয়:

1. **লক্ষ্য উপাদান** (target element) যখন ডিভাইসের **ভিউপোর্ট** বা নির্দিষ্ট উপাদানের (যাকে **root element** বলা হয়) সাথে ইন্টারসেক্ট করে।
2. যখন প্রথমবারের মতো observer কে লক্ষ্য উপাদানটি পর্যবেক্ষণ করতে বলা হয়।

সাধারণভাবে, আপনি লক্ষ্য উপাদানের **সর্বশেষ স্ক্রলযোগ্য পূর্বসূরি উপাদান** বা যদি লক্ষ্য উপাদানটি কোন স্ক্রলযোগ্য উপাদানের মধ্যে না থাকে, তবে ডিভাইসের ভিউপোর্টের সাথে ইন্টারসেকশন পর্যবেক্ষণ করতে চাইবেন। ডিভাইসের ভিউপোর্টের সাথে ইন্টারসেকশন ট্র্যাক করতে, `root` অপশনে `null` নির্ধারণ করুন। আরো বিস্তারিত ব্যাখ্যার জন্য নিচে পড়তে থাকুন।

আপনি যখন ভিউপোর্ট বা অন্য কোন উপাদানকে root হিসেবে ব্যবহার করবেন, তখন API একইভাবে কাজ করবে, অর্থাৎ যখন লক্ষ্য উপাদানটি root-এর সাথে প্রয়োজনীয় পরিমাণে ইন্টারসেক্ট করবে, তখন callback function চালু হবে।

**ইন্টারসেকশন রেশিও** (intersection ratio) হল লক্ষ্য উপাদান এবং তার root-এর মধ্যে ইন্টারসেকশনের পরিমাণ, যা 0.0 থেকে 1.0 এর মধ্যে একটি মান দিয়ে প্রকাশিত হয়। এটি লক্ষ্য উপাদানের কতটুকু অংশ দৃশ্যমান তা নির্দেশ করে।

### Intersection Observer তৈরি করা

Intersection observer তৈরি করতে, তার কনস্ট্রাক্টর কল করতে হয় এবং একটি callback ফাংশন পাস করতে হয় যা observer যখন ইন্টারসেকশন পেরিয়ে যায়, তখন চালু হবে:

```js
const options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

const observer = new IntersectionObserver(callback, options);
```

এখানে `threshold: 1.0` মানে হল যে, লক্ষ্য উপাদানের 100% অংশ যখন `root`-এ দৃশ্যমান হবে, তখন callback ফাংশন চালু হবে।

### Intersection Observer অপশনসমূহ

IntersectionObserver() কনস্ট্রাক্টরে পাস করা `options` অবজেক্টটি আপনাকে observer-এর callback কখন চালু হবে তা নিয়ন্ত্রণ করতে সাহায্য করে। এতে তিনটি প্রধান ফিল্ড রয়েছে:

1. **root**:  
   এটি সেই উপাদান যা লক্ষ্য উপাদানটির দৃশ্যমানতা পরীক্ষা করতে ব্যবহার করা হবে। এটি লক্ষ্য উপাদানের পূর্বসূরি হতে হবে। যদি নির্ধারণ না করা হয়, বা `null` দেওয়া হয়, তবে এটি ডিফল্টভাবে ব্রাউজারের ভিউপোর্ট হবে।

2. **rootMargin**:  
   root-এর চারপাশে মার্জিন। এটি CSS `margin` প্রোপার্টির মতো একটি স্ট্রিং, যেমন `"10px 20px 30px 40px"` (উপরে, ডানে, নিচে, বামে)। এই মানগুলো শূন্য বা পজিটিভ হতে পারে, এবং এটি root-এর সীমানা বেড়ে বা কমে যাবে। ডিফল্ট মান হলো `"0px 0px 0px 0px"`।

3. **threshold**:  
   এটি একক মান বা একাধিক সংখ্যার অ্যারে হতে পারে, যা লক্ষ্য উপাদানটির দৃশ্যমানতার যে শতাংশে observer-এর callback চালু হবে তা নির্দেশ করে। উদাহরণস্বরূপ:
   - `threshold: 0.5` মানে, যখন লক্ষ্য উপাদানটির 50% দৃশ্যমান হবে, তখন callback চালু হবে।
   - যদি আপনি চান যে callback ফাংশনটি প্রতি 25% দৃশ্যমানতার সাথে চালু হোক, তাহলে আপনি অ্যারে ব্যবহার করতে পারেন, যেমন: `[0, 0.25, 0.5, 0.75, 1]`।
   - ডিফল্ট হল `0` (অর্থাৎ, মাত্র একটি পিক্সেল দৃশ্যমান হলেই callback চলবে)। `1.0` মানে যে observer কেবল তখনই callback চালাবে যখন লক্ষ্য উপাদানটি সম্পূর্ণভাবে দৃশ্যমান হবে।

এভাবে, Intersection Observer API ব্যবহার করে আপনি ওয়েবপৃষ্ঠায় উপাদানগুলির দৃশ্যমানতা খুব সহজে ট্র্যাক করতে পারেন এবং ব্যবহারকারীর অভিজ্ঞতা উন্নত করতে পারেন।


**Intersection Change Callbacks:**

**IntersectionObserver** যখন একটি লক্ষ্য উপাদান ইন্টারসেক্ট করে (যা নির্দিষ্ট থ্রেশহোল্ড পাস করে), তখন এটি একটি **callback function** কল করে। এই callback-এ **IntersectionObserverEntry** অবজেক্টগুলির একটি তালিকা পাঠানো হয়, যা প্রতিটি পরিবর্তনের বিস্তারিত তথ্য ধারণ করে।

### Callback Function:
আপনি যখন `IntersectionObserver()` কনস্ট্রাক্টর কল করেন, তখন যে callback ফাংশনটি পাস করবেন, এটি একটি তালিকা পাবে যার মধ্যে থাকবে **IntersectionObserverEntry** অবজেক্টগুলো। এই অবজেক্টগুলো প্রতি এক্সেসযোগ্য লক্ষ্য উপাদানের ইন্টারসেকশন পরিবর্তন সম্পর্কে তথ্য দেয়। 

নিচে একটি সাধারণ callback ফাংশন দেখানো হলো:

```js
const callback = (entries, observer) => {
  entries.forEach((entry) => {
    // প্রতিটি entry এক্সপ্লেইন করে একটি লক্ষ্য উপাদানটির ইন্টারসেকশন পরিবর্তন:
    //   entry.boundingClientRect
    //   entry.intersectionRatio
    //   entry.intersectionRect
    //   entry.isIntersecting
    //   entry.rootBounds
    //   entry.target
    //   entry.time
  });
};
```

**entries** তালিকায় যেসব **IntersectionObserverEntry** অবজেক্ট থাকে, সেগুলোর মাধ্যমে আপনি জানতে পারবেন কতোবার একটি লক্ষ্য উপাদান নির্দিষ্ট থ্রেশহোল্ড অতিক্রম করেছে। একাধিক **entry** একসাথে আসতে পারে, যা একাধিক লক্ষ্য উপাদান বা একটিই লক্ষ্য উপাদান থেকে আসতে পারে, যা একাধিক থ্রেশহোল্ড পেরিয়েছে একসাথে।

**তথ্য গুলোর মধ্যে:**
- **boundingClientRect**: লক্ষ্য উপাদানের সীমানা।
- **intersectionRatio**: লক্ষ্য উপাদানটি কতটুকু দৃশ্যমান (0 থেকে 1 পর্যন্ত মান)।
- **intersectionRect**: লক্ষ্য উপাদান এবং root-এর মধ্যে ইন্টারসেকশন সীমানা।
- **isIntersecting**: লক্ষ্য উপাদানটি ইন্টারসেক্ট করছে কি না।
- **rootBounds**: root উপাদানের সীমানা।
- **target**: লক্ষ্য উপাদান।
- **time**: ইন্টারসেকশন ঘটার সময়।

### Callback Execution:
আপনার callback ফাংশনটি মূল থ্রেডে কার্যকর হবে, তাই এটি যতটা সম্ভব দ্রুত কাজ করা উচিত। যদি কিছু সময়সাপেক্ষ কাজ করতে হয়, তাহলে **Window.requestIdleCallback()** ব্যবহার করতে পারেন।

### উদাহরণ:
নিচে একটি উদাহরণ দেওয়া হলো যেখানে একটি কাউন্টার রাখা হয়েছে, যা গণনা করে কতবার লক্ষ্য উপাদানটি root-এর সাথে 75% দৃশ্যমান হতে ইন্টারসেক্ট করেছে:

```js
const intersectionCallback = (entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {  // লক্ষ্য উপাদানটি ইন্টারসেক্ট করছে কিনা চেক
      let elem = entry.target;

      if (entry.intersectionRatio >= 0.75) {  // 75% দৃশ্যমান হলে
        intersectionCounter++;  // কাউন্টার বাড়িয়ে দিন
      }
    }
  });
};
```

এখানে:
- `entry.isIntersecting` চেক করে যদি লক্ষ্য উপাদানটি ইন্টারসেক্ট করে।
- `entry.intersectionRatio` চেক করে যদি লক্ষ্য উপাদানটি 75% দৃশ্যমান হয়।
- তারপর কাউন্টারটি বাড়ানো হয়।


**Intersection Observer API** ব্যবহার করে আপনি ওয়েব পৃষ্ঠায় উপাদানের দৃশ্যমানতা পরিবর্তন ট্র্যাক করতে পারেন। এর মাধ্যমে আপনি অনেক সময়ের সাশ্রয় করতে পারেন এবং ওয়েবসাইটের পারফরম্যান্স উন্নত করতে পারেন।

**Targeting an Element to be Observed (লক্ষ্য উপাদান নির্বাচন করা)**

একবার আপনি **Intersection Observer** তৈরি করলে, আপনাকে একটি লক্ষ্য উপাদান (target element) সেট করতে হবে যেটি পর্যবেক্ষণ করা হবে:

```js
const target = document.querySelector("#listItem");
observer.observe(target);
```

এখন, observer এর জন্য যে callback ফাংশনটি আপনি সেট করেছেন তা প্রথমবারের মতো চালু হবে, এবং এটি লক্ষ্য উপাদানটি পর্যবেক্ষণ শুরু করবে (যদিও লক্ষ্য উপাদানটি দৃশ্যমান না হলেও)। 

যতবার লক্ষ্য উপাদান নির্দিষ্ট থ্রেশহোল্ড পাস করবে, callback ফাংশনটি চালু হবে।

**মনে রাখবেন:**  
যদি আপনি **root** অপশন নির্ধারণ করে থাকেন, তবে লক্ষ্য উপাদানটি অবশ্যই root উপাদানের একটি অঙ্গ (descendant) হতে হবে।

---

### **Intersection কিভাবে হিসাব করা হয়**

**Intersection Observer API** তে সব কিছু রেকট্যাঙ্গুলার (চতুর্ভুজ) হিসেবে গণনা করা হয়। যদি কোন উপাদান অস্বাভাবিক আকারের হয়, তবে তাকে সর্বনিম্ন রেকট্যাঙ্গুলার আকার হিসেবে গণ্য করা হয় যা উপাদানের সব অংশকে অন্তর্ভুক্ত করে।

যদি লক্ষ্য উপাদানের দৃশ্যমান অংশ রেকট্যাঙ্গুলার না হয়, তবে তার **intersection rectangle** হিসেবে মনে করা হয় যে এটি সেই সর্বনিম্ন রেকট্যাঙ্গুলার আকার, যা উপাদানের সমস্ত দৃশ্যমান অংশ ধারণ করে।

### **Intersection Root এবং Root Margin**

আমরা যখন একটি উপাদান এবং একটি কন্টেইনারের (যাকে **intersection root** বলা হয়) ইন্টারসেকশন ট্র্যাক করতে চাই, তখন প্রথমে কন্টেইনারটি জানতে হবে। এই কন্টেইনারটি **intersection root** হতে পারে, যা হয়তো একটি নির্দিষ্ট উপাদান হতে পারে যা লক্ষ্য উপাদানের পূর্বসূরি (ancestor) অথবা `null` হতে পারে, যেখানে ডিফল্ট কন্টেইনার হিসেবে ডকুমেন্টের ভিউপোর্ট ব্যবহার করা হবে।

**Root Intersection Rectangle** হলো সেই রেকট্যাঙ্গুলার যা লক্ষ্য উপাদানটির সাথে তুলনা করা হয়। এটি কিভাবে নির্ধারণ করা হয়:

- যদি intersection root ডিফল্ট root (মানে, শীর্ষ স্তরের ডকুমেন্ট) হয়, তাহলে root intersection rectangle হবে ভিউপোর্টের রেকট্যাঙ্গুলার।
- যদি intersection root-এর একটি overflow clip থাকে, তাহলে root intersection rectangle হবে root উপাদানের কন্টেন্ট এরিয়া।
- অন্যথায়, এটি হবে root উপাদানের **bounding client rectangle**, যা `getBoundingClientRect()` দ্বারা নির্ধারিত হবে।

এছাড়া, **rootMargin** অপশন দিয়ে root-এর সীমানাকে আরও বাড়ানো বা ছোটানো যেতে পারে। যদি rootMargin এ পজিটিভ মান থাকে, তাহলে বক্সটি বড় হবে; আর নেগেটিভ মান থাকলে, বক্সটি ছোট হয়ে যাবে।

---

### **Thresholds (থ্রেশহোল্ড)**

**Intersection Observer API** প্রত্যেকটি ক্ষুদ্র দৃশ্যমানতার পরিবর্তন রিপোর্ট না করে, বরং **threshold** ব্যবহার করে। আপনি observer তৈরি করার সময় লক্ষ্য উপাদানের দৃশ্যমানতার বিভিন্ন শতাংশ নির্ধারণ করতে পারেন। এরপর, API শুধুমাত্র সেই পরিবর্তনগুলো রিপোর্ট করবে যা এই থ্রেশহোল্ড পাস করবে।

উদাহরণস্বরূপ, যদি আপনি চান যে প্রতি ২৫% দৃশ্যমানতার পরিবর্তনে অবহিত হোন, তাহলে observer তৈরি করার সময় আপনি থ্রেশহোল্ডের তালিকা হিসেবে `[0, 0.25, 0.5, 0.75, 1]` ব্যবহার করবেন।

যখন callback ফাংশনটি চালু হবে, তখন এটি **IntersectionObserverEntry** অবজেক্টের একটি তালিকা পাবে, যার প্রতিটি অবজেক্ট লক্ষ্য উপাদানটি root-এর সাথে কতটুকু ইন্টারসেক্ট করছে এবং থ্রেশহোল্ড পাস করেছে কিনা তা জানাবে।

আপনি **isIntersecting** প্রপার্টি দেখে জানতে পারবেন যে লক্ষ্য উপাদানটি root-এর সাথে ইন্টারসেক্ট করছে কিনা। যদি `isIntersecting` সত্য হয়, তবে এটি নির্দেশ করে যে লক্ষ্য উপাদানটি অন্তত কিছুটা root উপাদানটির সাথে ইন্টারসেক্ট করছে।

---

### **Clipping এবং Intersection Rectangle**

ব্রাউজারটি **intersection rectangle** হিসাব করার জন্য নিচের পদক্ষেপগুলো অনুসরণ করে:

1. লক্ষ্য উপাদানটির **bounding rectangle** (`getBoundingClientRect()` ব্যবহার করে) প্রথমে নেওয়া হয়।
2. এরপর, লক্ষ্য উপাদানের প্রতিটি পিতৃ উপাদানের ক্লিপিং চেক করা হয়। যদি কোন ক্লিপিং থাকে (যেমন overflow প্রপার্টি দ্বারা), তবে তা intersection rectangle থেকে বাদ দেওয়া হয়।
3. যদি একটি **iframe** থাকে, তবে intersection rectangle শুধুমাত্র iframe এর ভিউপোর্ট পর্যন্ত ক্লিপ হয়, এবং পরবর্তী পিতৃ উপাদানগুলির দিকে রেকার্সিভভাবে চেক করা হয়।
4. যখন root উপাদানে পৌঁছানো হয়, তখন intersection rectangle সংশোধন করা হয়।
5. অবশেষে, এটি লক্ষ্য উপাদানের ডকুমেন্টের কনটেক্সটে মানানসই হয়।

এই সবগুলো পদক্ষেপ আপনার জন্য স্বয়ংক্রিয়ভাবে সম্পন্ন হয়, তবে এগুলোর কাজ বুঝতে পারলে intersection এর কাজ বোঝা সহজ হবে।

---


**Intersection Observer API** ব্যবহার করে, আপনি সহজেই ওয়েবপৃষ্ঠায় উপাদানের দৃশ্যমানতা পরিবর্তন ট্র্যাক করতে পারেন, যা স্ক্রলিং এবং লাজি লোডিংয়ের মতো কাজকে আরও দক্ষ ও পারফরম্যান্ট করে তোলে।


**IntersectionObserver API - সহজ এবং স্পষ্ট ব্যাখ্যা **

### 1. **IntersectionObserver**:
`IntersectionObserver` হল Intersection Observer API এর মূল ইন্টারফেস। এটি এমন একটি অবজারভার তৈরি এবং পরিচালনা করার জন্য ব্যবহৃত হয় যা একাধিক টার্গেট এলিমেন্টের দৃশ্যমানতার পরিবর্তনগুলি পর্যবেক্ষণ করতে পারে। এই অবজারভারটি অ্যাসিঙ্ক্রোনাসভাবে এক বা একাধিক টার্গেট এলিমেন্ট এবং তাদের অভিভাবক এলিমেন্ট বা ডকুমেন্টের ভিউপোর্টের মধ্যে ইন্টারসেকশনের (অর্থাৎ, সন্নিবেশ) পরিবর্তন পর্যবেক্ষণ করতে পারে। অভিভাবক এলিমেন্ট বা ভিউপোর্টকে "রুট" বলা হয়।

### 2. **IntersectionObserverEntry**:
`IntersectionObserverEntry` একটি অবজেক্ট যা টার্গেট এলিমেন্ট এবং তার রুট কন্টেইনারের মধ্যে ইন্টারসেকশন সম্পর্কে তথ্য দেয়। এটি একটি নির্দিষ্ট সময়ের মধ্যে ঘটিত ইন্টারসেকশন বর্ণনা করে। এই ধরনের অবজেক্ট দুটি উপায়ে পাওয়া যেতে পারে:
- IntersectionObserver callback ফাংশনে ইনপুট হিসেবে।
- IntersectionObserver.takeRecords() মেথড দ্বারা।

### 3. **সহজ উদাহরণ**:
এই উদাহরণটি দেখাবে কিভাবে একটি টার্গেট এলিমেন্টের রঙ এবং স্বচ্ছতা পরিবর্তন হয় যখন এটি দৃশ্যমানতা বাড়ায় বা কমায়। 

### **HTML**:
এখানে একটি সাধারণ HTML কোড দেওয়া হয়েছে, যেখানে একটি `#box` এলিমেন্ট রয়েছে, যার মধ্যে কিছু কনটেন্ট রয়েছে।

```html
<div id="box">
  <div class="vertical">Welcome to <strong>The Box!</strong></div>
</div>
```

### **CSS**:
```css
#box {
  background-color: rgb(40 40 190 / 100%);
  border: 4px solid rgb(20 20 120);
  transition:
    background-color 1s,
    border 1s;
  width: 350px;
  height: 350px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.vertical {
  color: white;
  font: 32px "Arial";
}
```
এই CSS কোডটি টার্গেট এলিমেন্টের আকার, রঙ এবং অন্যান্য শৈলী নির্ধারণ করে এবং ট্রানজিশন প্রয়োগ করে যা এলিমেন্টের রঙ পরিবর্তন করতে সাহায্য করবে।

### **JavaScript**:
এখন আমরা JavaScript কোডটি দেখব, যা IntersectionObserver API ব্যবহার করে এলিমেন্টের দৃশ্যমানতা পর্যবেক্ষণ করবে এবং রঙ পরিবর্তন করবে।

#### **স্টেপ ১: অবজারভার সেটআপ**:
```js
const numSteps = 20.0;

let boxElement;
let prevRatio = 0.0;
let increasingColor = "rgb(40 40 190 / ratio)";
let decreasingColor = "rgb(190 40 40 / ratio)";

// Set things up
window.addEventListener("load", (event) => {
  boxElement = document.querySelector("#box");
  createObserver();
}, false);
```
- **numSteps**: এটি নির্ধারণ করে যে, দৃশ্যমানতার রেশিও 0.0 থেকে 1.0 পর্যন্ত কতটি স্তর হবে।
- **prevRatio**: এটি রেকর্ড রাখে যে শেষবার দৃশ্যমানতার রেশিও কত ছিল।
- **increasingColor** এবং **decreasingColor**: এই দুটি স্ট্রিং পরিবর্তনশীল রঙের কোড সরবরাহ করে, যা দৃশ্যমানতার পরিবর্তন অনুযায়ী এলিমেন্টে প্রয়োগ হবে।

#### **স্টেপ ২: IntersectionObserver তৈরি করা**:
```js
function createObserver() {
  let observer;
  
  let options = {
    root: null,
    rootMargin: "0px",
    threshold: buildThresholdList(),
  };

  observer = new IntersectionObserver(handleIntersect, options);
  observer.observe(boxElement);
}
```
এই ফাংশনটি `IntersectionObserver` তৈরি করে, যা `#box` এলিমেন্টের দৃশ্যমানতা পর্যবেক্ষণ করতে পারে। এর মধ্যে `rootMargin` এবং `threshold` ব্যবহার করে কাস্টমাইজ করা যায় কীভাবে ও কখন ইন্টারসেকশন পরিবর্তন হতে হবে।

#### **স্টেপ ৩: থ্রেশহোল্ড তালিকা তৈরি করা**:
```js
function buildThresholdList() {
  let thresholds = [];
  let numSteps = 20;

  for (let i = 1.0; i <= numSteps; i++) {
    let ratio = i / numSteps;
    thresholds.push(ratio);
  }

  thresholds.push(0);
  return thresholds;
}
```
এই ফাংশনটি একটি থ্রেশহোল্ড তালিকা তৈরি করে, যা 0.0 থেকে 1.0 পর্যন্ত দৃশ্যমানতার বিভিন্ন স্তর নির্ধারণ করে। এটি 20টি স্তরের মধ্যে বিভক্ত।

#### **স্টেপ ৪: Intersection পরিবর্তন পরিচালনা করা**:
```js
function handleIntersect(entries, observer) {
  entries.forEach((entry) => {
    if (entry.intersectionRatio > prevRatio) {
      entry.target.style.backgroundColor = increasingColor.replace(
        "ratio",
        entry.intersectionRatio
      );
    } else {
      entry.target.style.backgroundColor = decreasingColor.replace(
        "ratio",
        entry.intersectionRatio
      );
    }

    prevRatio = entry.intersectionRatio;
  });
}
```
যখন এলিমেন্টের দৃশ্যমানতা একটি নির্দিষ্ট স্তরে পৌঁছায়, তখন এই ফাংশনটি কল হয়। এতে রঙ পরিবর্তন করা হয় এবং টার্গেট এলিমেন্টের স্বচ্ছতাও পরিবর্তিত হয়।

### **ফলস্বরূপ**:
এখন, আপনি পেজটি স্ক্রল করলে দেখতে পাবেন যে `#box` এলিমেন্টটির রঙ এবং স্বচ্ছতা পরিবর্তিত হচ্ছে, যেমন এটি দৃশ্যমানতার স্তরের পরিবর্তন ঘটছে।


এই উদাহরণটি IntersectionObserver API ব্যবহার করে একটি এলিমেন্টের দৃশ্যমানতা পর্যবেক্ষণ করার পদ্ধতি এবং সেই অনুযায়ী রঙ এবং স্বচ্ছতা পরিবর্তন করার প্রক্রিয়া ব্যাখ্যা করেছে। এটি আপনার ওয়েবপেজে এলিমেন্টের দৃশ্যমানতা ট্র্যাক এবং কাস্টমাইজ করতে সহায়ক।

### Intersection Observer তৈরি করা - সহজভাবে ব্যাখ্যা

`createObserver()` মেথডটি তখন কল করা হয় যখন পেজ লোড সম্পূর্ণ হয়। এটি নতুন একটি `IntersectionObserver` তৈরি করতে সাহায্য করে এবং টার্গেট এলিমেন্ট পর্যবেক্ষণ শুরু করে।

#### কোড:
```js
function createObserver() {
  let observer;

  let options = {
    root: null,
    rootMargin: "0px",
    threshold: buildThresholdList(),
  };

  observer = new IntersectionObserver(handleIntersect, options);
  observer.observe(boxElement);
}
```

### ব্যাখ্যা:

1. **options অবজেক্ট**:
   এখানে প্রথমে `options` নামে একটি অবজেক্ট তৈরি করা হয়েছে, যার মধ্যে কিছু সেটিংস দেওয়া হয়েছে:
   - **root**: এখানে `root` মানে হচ্ছে ভিউপোর্ট বা অভিভাবক এলিমেন্ট, যার মধ্যে টার্গেট এলিমেন্টটির অবস্থান পরীক্ষা করা হবে। যদি এটিকে `null` রাখা হয়, তাহলে এটি ডকুমেন্টের ভিউপোর্টকেই রুট হিসেবে ধরে নিবে।
   - **rootMargin**: এই সেটিংটি ভিউপোর্টের চারপাশে কোনো অতিরিক্ত মার্জিন যোগ করার জন্য ব্যবহৃত হয়। এখানে `0px` রাখা হয়েছে, অর্থাৎ কোন অতিরিক্ত মার্জিন থাকবে না।
   - **threshold**: এই সেটিংটি বিভিন্ন দৃশ্যমানতার স্তরের (visibility ratio) জন্য থ্রেশহোল্ড তৈরি করে। এটা `buildThresholdList()` ফাংশন দ্বারা তৈরি হবে, যেটি বিভিন্ন স্তরের রেশিও নির্ধারণ করবে।

2. **IntersectionObserver তৈরি করা**:
   - `IntersectionObserver` কন্সট্রাকটর ব্যবহার করে একটি নতুন অবজারভার তৈরি করা হয়। এই অবজারভারটি `handleIntersect` ফাংশনটি কল করবে, যখন কোনো এলিমেন্ট নির্ধারিত থ্রেশহোল্ড পাস করবে বা ক্রস করবে।

3. **observe() মেথড**:
   অবজারভারটি তৈরি হওয়ার পর, `observe()` মেথডটি কল করা হয়, যা `boxElement` (টার্গেট এলিমেন্ট) কে পর্যবেক্ষণের জন্য নির্দেশ দেয়।

### লক্ষ্য:
এই কোডটি মূলত একটি `IntersectionObserver` তৈরি করে, যা নির্দিষ্ট শর্তে টার্গেট এলিমেন্টের দৃশ্যমানতা পরিবর্তন পর্যবেক্ষণ করবে এবং তখনই একটি নির্দিষ্ট ফাংশন কল করবে (যেমন `handleIntersect()` ফাংশন)। এটি আপনাকে এলিমেন্টের দৃশ্যমানতা এবং স্ক্রল অবস্থান অনুযায়ী অ্যাকশন নিতে সাহায্য করে।

### একাধিক এলিমেন্ট পর্যবেক্ষণ:
আপনি চাইলে একাধিক এলিমেন্টের দৃশ্যমানতা পর্যবেক্ষণ করতে পারেন। এজন্য শুধু `observer.observe()` মেথডটি অন্য এলিমেন্টের জন্যও কল করতে হবে।

এভাবে, `createObserver()` ফাংশনটি একটি নতুন অবজারভার তৈরি করে এবং টার্গেট এলিমেন্টের দৃশ্যমানতা পর্যবেক্ষণ করতে শুরু করে।



### থ্রেশহোল্ড রেশিওর অ্যারে তৈরি করা

`buildThresholdList()` ফাংশনটি একটি থ্রেশহোল্ডের তালিকা তৈরি করে, যা 0.0 এবং 1.0 এর মধ্যে বিভিন্ন রেশিও ব্যবহার করে। এই ফাংশনটি নিচের মতো কাজ করে:

#### কোড:
```js
function buildThresholdList() {
  let thresholds = [];
  let numSteps = 20;

  for (let i = 1.0; i <= numSteps; i++) {
    let ratio = i / numSteps;
    thresholds.push(ratio);
  }

  thresholds.push(0);
  return thresholds;
}
```

### ব্যাখ্যা:

1. **থ্রেশহোল্ড অ্যারে তৈরি**:
   এখানে একটি খালি অ্যারে `thresholds` তৈরি করা হয়েছে। এরপর `numSteps` নামে একটি ভ্যারিয়েবল দেয়া হয়েছে যার মান ২০, অর্থাৎ ২০টি ধাপে আমরা থ্রেশহোল্ড তৈরি করতে চাই।

2. **for লুপ ব্যবহার**:
   লুপের মাধ্যমে `i` এর মান ১ থেকে শুরু হয়ে `numSteps` (২০) পর্যন্ত চলতে থাকে। প্রতিটি `i` এর জন্য `i / numSteps` রেশিও গণনা করা হয় এবং সেই রেশিওটি `thresholds` অ্যারেতে যোগ করা হয়। 

3. **0 যুক্ত করা**:
   লুপ শেষ হওয়ার পর, `thresholds.push(0)` দিয়ে ০ থ্রেশহোল্ডটি অ্যারেতে যোগ করা হয়। এটি নিশ্চিত করে যে ০ রেশিওও তালিকায় থাকবে।

4. **ফলস্বরূপ অ্যারে**:
   যেহেতু `numSteps` ২০, তাই অ্যারেটি হবে ২১টি মানের (০ থেকে ১.০ পর্যন্ত), যার মধ্যে প্রতিটি রেশিও হবে:
   
এই সারণীতে থ্রেশহোল্ড রেশিওগুলির একটি তালিকা দেওয়া হয়েছে, যেখানে # মানে হচ্ছে থ্রেশহোল্ডের সংখ্যাগত অবস্থান এবং রেশিও মানে হচ্ছে সেই অবস্থানে এলিমেন্টের দৃশ্যমানতার পরিমাণ (০ থেকে ১ এর মধ্যে)। এর মাধ্যমে আমরা বুঝতে পারি কোন পজিশনে এলিমেন্ট কতটা দৃশ্যমান।

### থ্রেশহোল্ড রেশিওর তালিকা:

| #  | রেশিও (Ratio) |
|----|----------------|
| 0  | 0.05           |
| 1  | 0.1            |
| 2  | 0.15           |
| 3  | 0.2            |
| 4  | 0.25           |
| 5  | 0.3            |
| 6  | 0.35           |
| 7  | 0.4            |
| 8  | 0.45           |
| 9  | 0.5            |
| 10 | 0.55           |
| 11 | 0.6            |
| 12 | 0.65           |
| 13 | 0.7            |
| 14 | 0.75           |
| 15 | 0.8            |
| 16 | 0.85           |
| 17 | 0.9            |
| 18 | 0.95           |
| 19 | 1              |
| 20 | 0              |

### ব্যাখ্যা:
- **থ্রেশহোল্ড রেশিও**: এটি একটি মান যা এলিমেন্টের দৃশ্যমানতার পরিমাণ নির্দেশ করে। উদাহরণস্বরূপ, ০.৫ মানে হচ্ছে এলিমেন্টটি ৫০% দৃশ্যমান।
- **#**: এটি থ্রেশহোল্ডের অবস্থান বা সংখ্যা। এটি নির্ধারণ করে যে কতটুকু দৃশ্যমানতা পরিসরে এলিমেন্ট পৌঁছেছে।
  
### উদাহরণ:
- যদি রেশিও ০.৫ হয়, এর মানে হচ্ছে এলিমেন্টটি স্ক্রিনের ৫০% অংশ দৃশ্যমান।
- রেশিও ১ হলে এলিমেন্ট পুরোপুরি দৃশ্যমান, এবং ০ হলে তা সম্পূর্ণ অদৃশ্য।

এই রেশিওগুলির মাধ্যমে Intersection Observer API কাজ করে এবং আপনাকে এলিমেন্টের দৃশ্যমানতার স্তরের উপর ভিত্তি করে কার্যক্রম পরিচালনা করতে সাহায্য করে।
### অতিরিক্ত কনফিগারেশন:
আপনি চাইলে এই থ্রেশহোল্ড তালিকাটিকে কাস্টমাইজ করতে পারেন। যেমন, আরো কম বা বেশি ধাপে থ্রেশহোল্ড তৈরি করতে পারেন, অথবা এটি কোডে হার্ডকোড (manually set) করতে পারেন।

এইভাবে, `buildThresholdList()` ফাংশনটি একটি অ্যারে তৈরি করে যা ০ থেকে ১ পর্যন্ত বিভিন্ন থ্রেশহোল্ড রেশিও ধারণ করে, এবং সেটি `IntersectionObserver` এর জন্য ব্যবহৃত হয়।

**Intersection পরিবর্তন হ্যান্ডলিং**

যখন ব্রাউজার এটি শনাক্ত করে যে লক্ষ্য উপাদান (আমাদের ক্ষেত্রে, যেটি "box" আইডি ধারণ করে) দৃশ্যমান বা অস্পষ্ট হয়েছে এবং তার দৃশ্যমানতার রেশিও আমাদের তালিকার কোন থ্রেশহোল্ড পার করেছে, তখন এটি আমাদের হ্যান্ডলার ফাংশন **handleIntersect()** কে কল করে।

### কোড উদাহরণ:

```js
function handleIntersect(entries, observer) {
  entries.forEach((entry) => {
    if (entry.intersectionRatio > prevRatio) {
      entry.target.style.backgroundColor = increasingColor.replace(
        "ratio",
        entry.intersectionRatio,
      );
    } else {
      entry.target.style.backgroundColor = decreasingColor.replace(
        "ratio",
        entry.intersectionRatio,
      );
    }

    prevRatio = entry.intersectionRatio;
  });
}
```

### ব্যাখ্যা:
এই ফাংশনটি `IntersectionObserverEntry` তালিকার প্রতিটি উপাদান যাচাই করে দেখবে যে, `intersectionRatio` (যার মান ০ থেকে ১ এর মধ্যে হয়) আগের রেশিওর থেকে বেশি হচ্ছে কিনা। যদি রেশিও বৃদ্ধি পায়, তাহলে এলিমেন্টের পটভূমি রং পরিবর্তিত হবে এবং এটি আরও উজ্জ্বল বা অস্বচ্ছ হবে, কারণ আমরা পটভূমি রঙের সাথে রেশিও মান যুক্ত করেছি।

- **বর্ধিত রেশিও**: যদি দৃশ্যমানতা বাড়ে, তখন `increasingColor` এর মাধ্যমে পটভূমি রঙ পরিবর্তন হবে। এই রঙের মধ্যে "ratio" শব্দটি বর্তমান রেশিও দিয়ে প্রতিস্থাপিত হবে।
- **হ্রাসমান রেশিও**: যদি দৃশ্যমানতা কমে, তখন `decreasingColor` দিয়ে পটভূমি রঙ পরিবর্তন হবে। এখানে "ratio" শব্দটি বর্তমান রেশিও দ্বারা প্রতিস্থাপিত হবে।

এছাড়া, প্রতিটি পরিবর্তন ট্র্যাক করতে, আমরা `prevRatio` ভ্যারিয়েবলে সর্বশেষ রেশিওটি সংরক্ষণ করি।

### ফলাফল:
যখন আপনি পেজটি উপরে বা নিচে স্ক্রোল করবেন, তখন আপনি লক্ষ্য করবেন যে "box" উপাদানের রঙ এবং স্বচ্ছতা পরিবর্তিত হচ্ছে, যেহেতু এটি তার দৃশ্যমানতার রেশিও অনুযায়ী পরিবর্তিত হচ্ছে।

এই প্রক্রিয়াটি বিভিন্ন উপাদানের দৃশ্যমানতার উপর নির্ভর করে তাদের রঙ, স্বচ্ছতা বা অন্যান্য গুণাবলী পরিবর্তন করার জন্য ব্যবহার করা যেতে পারে।