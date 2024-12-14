# 鉴权

## 前端

* 前端通过middleware控制，若无token则不放行

  * 场景：用户直接在地址栏输入/dashboard ，由于没有token返回到login界面

  ```typescript
  import { NextResponse } from 'next/server'
  import type { NextRequest } from 'next/server'
  
  export function middleware(request: NextRequest) {
    const TOKEN_KEY = 'access_token';
    const token = request.cookies.get(TOKEN_KEY)
    const { pathname } = request.nextUrl
  
    const publicPaths = ['/auth/login', '/auth/register']
    
    if (!token && !publicPaths.includes(pathname)) {
      return NextResponse.redirect(new URL('/auth/login', request.url))
    }
  
    return NextResponse.next()
  }
  
  export const config = {
    matcher: [
      '/((?!api|_next/static|_next/image|favicon.ico).*)',
    ],
  }
  ```

## 后端

