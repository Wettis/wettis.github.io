---
title: "Hello World Next.js"
date: 2023-11-17T12:12:48+08:00
categories: []
draft: true
---

> Platform： Windows 11 Profession
>
> IDE ： WebStorm
>
> Doc： [Next Learn](https://nextjs.org/learn)

## 安装与运行

通过该命令安装 dashboard 项目

```cmd
npx create-next-app@latest nextjs-dashboard --use-npm --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example"
```

观察下目录结构 具体介绍参照下[文档](https://nextjs.org/learn/dashboard-app/getting-started#folder-structure)

能使用 `npm run dev` 就算成功

## 使用 CSS

### 添加全局css 

我们需要将`global.css` 引入到 root layout 中，在app路由下 我们只需要在 `/app/layout.tsx` 的头部添加代码 `import '@/app/ui/global.css'` 重新观察页面 页面将正常加载

### Tailwind

大概就是说使用类名就能很方便的为网页元素添加样式

### CSS Modules

创建 `/app/ui/home.module.css` 文件 为其添加内容

```css
.shape {
    height: 0;
    width: 0;
    border-bottom: 30px solid black;
    border-left: 20px solid transparent;
    border-right: 20px solid transparent;
}
```

在`/app/page.tsx` 文件顶部中引入上述文件

添加下方代码块第四行内容

``` tsx
        <div className="flex flex-col justify-center gap-6 rounded-lg bg-gray-50 px-6 py-10 md:w-2/5 md:px-20">
          <p className={`text-xl text-gray-800 md:text-3xl md:leading-normal`}>
            {/* 引入样式 */}
            <div className={styles.shape}></div>;        
```

返回页面就能看见同样有个黑色三角

CSS Modules 就是这样子使用，并且能与 Tailwind 共存。

### 使用 clsx 库去切换 class name

```tsx
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    {/* 
    	如果 status 为 pending 那么我们就希望他是绿色
    	如果 status 为 paid 那么我们就希望他是灰色
    */}
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```

### Other

除了这些提到的方式之外 同时还支持我们自己去使用 Sass 或者 CSS-in-JS libraries 比方说 [styled-jsx](https://github.com/vercel/styled-jsx), [styled-components](https://github.com/vercel/next.js/tree/canary/examples/with-styled-components)和[emotion](https://github.com/vercel/next.js/tree/canary/examples/with-emotion).

## 字体与图片

大致就是介绍了一下 `Next.js` 与其他框架不同的是，他的字体文件将会在页面创建之初与其他静态资源文件一起被载到用户浏览器中，而并非采用了其他网站实现的异步加载其他字体由此达到更好的体验效果。 

### 添加字体

运行  `cnpm i @next/font` 

创建 `/app/ui/fonts.ts` 文件 ，添加以下内容

```ts
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

在 `/app/layout.tsx` 文件中 添加 2 行，修改 11 行内容

``` tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      {/* 
    		antialiased 是由 tailwind 提供的平滑字体
    	*/}
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

### 练习

参考文档 [Doc-fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)

#### 本地字体

[Doc](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts) 暂时不展开了 有兴趣再去了解

#### Google 字体

挑选了字体 [Noto Sans Simplified Chinese](https://fonts.google.com/noto/specimen/Noto+Sans+SC?subset=chinese-simplified&noto.script=Hans)

在 `/app/ui/fonts.ts` 引入 如下

```ts
import { Inter,Noto_Serif_SC } from 'next/font/google';
// 1.如果字体文件中间为空，使用下划线代替
// 2.Simplified Chinese 被简写成 SC 其他语言类似

export const inter = Inter({ subsets: ['latin'] });

export const notoserif = Noto_Serif_SC({
    subsets: ['latin'],
    variable: '--font-notoserif',
    weight: "400"
})
```

### 图片

首先需要将图片放入 `/public` 内，应用程序就能引用到。

Next js 它封装了 `<Image>` 组件，大概就是为不同大小的屏幕做了适配，具体不展开需要去了解。

## 布局与页面路由

使用 `layout` 添加多个页面共享的部分，可以使其余页面部分渲染，而不是全部渲染。

## 在页面之间导航

### Link

next 同样封装了 `Link` 组件

只要页面上出现的`Link`组件那么页面将会被预载，使得页面的加载用户完全感知不到。

### usePathname( )

这是一个next独有的钩子函数，仅限于客户端的钩子函数，你需要在使用的文件顶部预先声明`'use client';`

```tsx
// 获取路径名称
const pathname = usePathname();
```



<hr>

记录下一些常用库

| Repository                                                   | Explanatory                                              |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| [Lodash.js](https://www.lodashjs.com/)                       | 常用工具库                                               |
| [cross-env](https://www.npmjs.com/package/cross-env)         | 运行跨平台设置的和使用环境变量（Node中的环境变量）的脚本 |
| [class-transformer](https://github.com/typestack/class-transformer) | 对象和类之间基于装饰器的转换、序列化和反序列化           |
| [class-validator](https://github.com/typestack/class-validator) | 基于装饰器的类属性验证。                                 |
| [typeorm](https://typeorm.io/)                               | ts  编写的 *node.js* *orm*                               |
| [Fastify](https://docs.nestjs.com/)                          | 更换 *nest.js* 中默认的 web 服务器为 *fastify*           |
|                                                              |                                                          |
|                                                              |                                                          |
|                                                              |                                                          |

