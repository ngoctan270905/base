# Mobile Cross App Base

Đây là base project cho app mobile cross-platform dùng **Expo + React Native + TypeScript**.

Mục tiêu của base này là dùng làm nền cho dự án production, không phải demo học tập. Project được tổ chức theo hướng **feature-based architecture** để dễ mở rộng, dễ bảo trì và giảm rối khi app lớn dần.

## Tech stack

```text
Expo SDK 54              // nền tảng build/chạy app React Native
React Native 0.81        // framework mobile cross-platform
React 19                 // UI library
TypeScript               // type-safe code
React Navigation         // điều hướng màn hình
Safe Area Context        // xử lý vùng an toàn trên iOS/Android
```

## Cài đặt

Cài dependency:

```bash
npm install
```

Tạo file môi trường local:

```bash
cp .env.example .env
```

Chạy app:

```bash
npx expo start
```

Nếu Metro/Expo bị cache import cũ sau khi đổi/xoá file:

```bash
npx expo start -c
```

## Environment variables

File mẫu nằm ở:

```text
.env.example             // file ví dụ env, được commit lên git
.env                     // file env thật ở máy local, không nên commit
```

Expo chỉ expose biến có prefix `EXPO_PUBLIC_` vào client app:

```env
EXPO_PUBLIC_APP_ENV=development
EXPO_PUBLIC_API_URL=https://api.example.com
```

Lưu ý:

```text
EXPO_PUBLIC_*            // biến public, có thể bị thấy trong app bundle
SECRET_KEY               // không nên để trong mobile client
DATABASE_PASSWORD        // không bao giờ để trong mobile client
PRIVATE_API_KEY          // không bao giờ để trong mobile client
```

App mobile có thể bị inspect bundle, nên mọi thứ ship vào app phải được xem như public.

## Cấu trúc thư mục

```text
my-app/
├── App.tsx                              // entry component chính, chỉ compose providers + navigator
├── index.ts                             // đăng ký root component với Expo
├── app.json                             // cấu hình Expo app
├── package.json                         // scripts và dependencies
├── package-lock.json                    // lock dependency version
├── tsconfig.json                        // cấu hình TypeScript
├── .env.example                         // env mẫu cho developer
├── .gitignore                           // danh sách file/folder không commit
├── CLAUDE.md                            // instruction local cho Claude Code
├── AGENTS.md                            // rule/instruction bổ sung cho agent
├── README.md                            // tài liệu tổng quan project
│
├── src/                                 // toàn bộ source code chính của app
│   ├── app/                             // tầng khởi tạo app cấp cao
│   │   └── providers/                   // gom các provider toàn app
│   │       └── AppProviders.tsx         // nơi bọc AuthProvider, ThemeProvider, QueryProvider sau này
│   │
│   ├── navigation/                      // cấu hình điều hướng app
│   │   ├── RootNavigator.tsx            // navigator gốc của app
│   │   └── navigationTypes.ts           // type cho route params của React Navigation
│   │
│   ├── features/                        // code chia theo tính năng/nghiệp vụ
│   │   ├── home/                        // feature trang chủ mẫu
│   │   │   ├── index.ts                 // public exports của feature home
│   │   │   ├── screens/                 // màn hình thuộc feature home
│   │   │   │   └── HomeScreen.tsx       // màn hình trang chủ mẫu
│   │   │   ├── components/              // component chỉ dùng riêng trong home
│   │   │   └── hooks/                   // hook chỉ dùng riêng trong home
│   │   │
│   │   └── auth/                        // feature auth, chuẩn bị cho login/register sau này
│   │       ├── screens/                 // LoginScreen, RegisterScreen, ForgotPasswordScreen
│   │       ├── components/              // LoginForm, RegisterForm, SocialLoginButtons
│   │       ├── hooks/                   // useLogin, useRegister, useAuthForm
│   │       ├── services/                // authApi, authService riêng auth
│   │       ├── schemas/                 // validation schema cho form auth
│   │       └── types/                   // type riêng của auth
│   │
│   ├── shared/                          // code UI dùng chung nhiều feature
│   │   ├── components/                  // AppButton, AppInput, AppText, LoadingView, EmptyState
│   │   └── layouts/                     // layout dùng chung như ScreenLayout, AuthLayout
│   │
│   ├── services/                        // tầng giao tiếp bên ngoài app
│   │   ├── api/                         // apiClient, interceptors, apiError, apiConfig
│   │   ├── storage/                     // AsyncStorage/SecureStore wrapper, storageKeys
│   │   ├── permissions/                 // helper xin quyền camera, location, media
│   │   ├── analytics/                   // wrapper analytics event tracking
│   │   ├── notifications/               // setup push/local notifications
│   │   └── logger/                      // logging/error reporting wrapper
│   │
│   ├── store/                           // global client state
│   │   └── index.ts                     // export store sau này nếu dùng Zustand/Redux/Jotai
│   │
│   ├── hooks/                           // custom hooks dùng chung toàn app
│   │   └── index.ts                     // export shared hooks sau này
│   │
│   ├── lib/                             // config/wrapper cho thư viện bên ngoài
│   │   └── index.ts                     // queryClient, sentry, i18n, validation config sau này
│   │
│   ├── constants/                       // hằng số dùng chung
│   │   └── colors.ts                    // bảng màu cơ bản của app
│   │
│   ├── types/                           // type global dùng chung toàn app
│   │   └── index.ts                     // export global types sau này
│   │
│   ├── utils/                           // helper function thuần logic
│   │   └── index.ts                     // formatDate, validateEmail, formatCurrency sau này
│   │
│   ├── config/                          // cấu hình runtime/build
│   │   └── index.ts                     // env, featureFlags, appConfig sau này
│   │
│   └── assets/                          // assets nằm trong source tree
│       ├── images/                      // ảnh png/jpg/webp
│       ├── icons/                       // icon app dùng trong UI
│       ├── fonts/                       // custom fonts
│       └── animations/                  // animation json/lottie
│
├── docs/                                // tài liệu nội bộ dự án
│   ├── architecture.md                  // mô tả kiến trúc app sau này
│   ├── api.md                           // ghi chú API contract sau này
│   └── release.md                       // quy trình release sau này
│
├── scripts/                             // script tự động hóa project
│   ├── clean.js                         // script clean cache/build sau này
│   ├── generate-env.js                  // script generate env sau này
│   └── release.js                       // script hỗ trợ release sau này
│
├── __tests__/                           // unit/component/integration tests
│   ├── utils/                           // test helper function
│   ├── services/                        // test service/api/storage
│   └── components/                      // test shared component
│
├── e2e/                                 // end-to-end tests
│   ├── login.e2e.ts                     // test flow login sau này
│   ├── onboarding.e2e.ts                // test flow onboarding sau này
│   └── checkout.e2e.ts                  // test flow checkout sau này
│
└── .github/                             // cấu hình GitHub automation
    └── workflows/                       // CI/CD workflows
        ├── typecheck.yml                // workflow chạy TypeScript check sau này
        ├── lint.yml                     // workflow chạy lint sau này
        ├── test.yml                     // workflow chạy test sau này
        └── build.yml                    // workflow build app sau này
```

## Nguyên tắc kiến trúc

### 1. `App.tsx` phải gọn

`App.tsx` không nên chứa screen, API logic, form logic hay business logic.

Nó chỉ nên giống như:

```tsx
import { AppProviders } from './src/app/providers/AppProviders';
import { RootNavigator } from './src/navigation/RootNavigator';

export default function App() {
  return (
    <AppProviders>
      <RootNavigator />
    </AppProviders>
  );
}
```

### 2. Code theo feature

Nếu code thuộc nghiệp vụ cụ thể, đặt trong:

```text
src/features/<feature-name>/
```

Ví dụ:

```text
src/features/auth/                 // login, register, forgot password
src/features/profile/              // profile user
src/features/product/              // danh sách/chi tiết sản phẩm
src/features/order/                // đơn hàng
src/features/settings/             // cài đặt app
```

### 3. Component dùng chung để trong `shared`

Nếu component chỉ dùng trong một feature, để trong feature đó:

```text
src/features/auth/components/LoginForm.tsx
```

Nếu component dùng nhiều nơi, đưa vào shared:

```text
src/shared/components/AppButton.tsx
src/shared/components/AppInput.tsx
src/shared/components/LoadingView.tsx
```

### 4. Không gọi API lung tung trong screen

Không nên viết trực tiếp `fetch` trong screen khi app lên production.

Nên đi theo hướng:

```text
Screen -> Hook -> Service -> ApiClient
```

Ví dụ:

```text
ProductListScreen.tsx              // UI màn hình
useProducts.ts                     // quản lý loading/error/data
productApi.ts                      // gọi API product
apiClient.ts                       // config base URL, headers, interceptors
```

### 5. Type route phải rõ ràng

Khi thêm screen mới, cập nhật:

```text
src/navigation/navigationTypes.ts
```

Ví dụ:

```ts
export type RootStackParamList = {
  Home: undefined;
  ProductDetail: {
    productId: string;
  };
};
```

Điều này giúp `navigation.navigate()` type-safe hơn.

### 6. Constants không hard-code lung tung

Màu sắc, spacing, typography nên đưa vào:

```text
src/constants/
```

Ví dụ hiện tại:

```text
src/constants/colors.ts
```

Sau này có thể thêm:

```text
src/constants/spacing.ts
src/constants/typography.ts
src/constants/radius.ts
src/constants/storageKeys.ts
```

### 7. Env không chứa secret

Mobile app là client app. Không để secret trong `.env` nếu secret đó ship vào app.

Không nên:

```env
EXPO_PUBLIC_SECRET_KEY=abc
EXPO_PUBLIC_DATABASE_PASSWORD=abc
EXPO_PUBLIC_PRIVATE_API_KEY=abc
```

Chỉ nên để các giá trị public như:

```env
EXPO_PUBLIC_APP_ENV=development
EXPO_PUBLIC_API_URL=https://api.example.com
```

## Quy ước thêm feature mới

Ví dụ thêm feature `profile`:

```text
src/features/profile/
├── index.ts                         // export public API của feature
├── screens/
│   └── ProfileScreen.tsx            // màn hình profile
├── components/
│   └── ProfileHeader.tsx            // component riêng profile
├── hooks/
│   └── useProfile.ts                // hook riêng profile
├── services/
│   └── profileApi.ts                // API riêng profile
├── types/
│   └── profileTypes.ts              // type riêng profile
└── schemas/
    └── updateProfileSchema.ts       // validation schema nếu có form
```

Sau đó thêm route vào:

```text
src/navigation/navigationTypes.ts
src/navigation/RootNavigator.tsx
```

## Scripts thường dùng

```bash
npm install                         # cài dependency
npx expo start                      # chạy Expo dev server
npx expo start -c                   # chạy Expo và clear cache
npx tsc --noEmit                    # kiểm tra TypeScript
```

## Ghi chú production

Trước khi đưa app lên production, nên bổ sung dần:

```text
Lint                                // kiểm tra code style
Formatter                           // format code tự động
Unit tests                          // test logic nhỏ
Component tests                     // test component dùng chung
E2E tests                           // test flow quan trọng
Error tracking                      // Sentry hoặc service tương tự
Analytics                           // tracking event quan trọng
CI/CD                               // tự động typecheck/test/build
EAS Build                           // build Android/iOS production
```
