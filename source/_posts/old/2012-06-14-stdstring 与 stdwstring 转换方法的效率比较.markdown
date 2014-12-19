---
layout: post
title: "std::string 与 std::wstring 转换方法的效率比较"
date: 2012-06-14 13:18:00 
comments: true
categories: [Windows 8]
tags: [Windows 8]
description: "std::string 与 std::wstring 转换方法的效率比较"
keywords: Windows 8
---

 // Calls the provided work function and returns the number of milliseconds 
// that it takes to call that function.
template <class Function>;
__int64 time_call(Function&& f)
{
    __int64 begin = GetTickCount64();
    f();
    return GetTickCount64() - begin;
}
// Use WideCharToMultiByte and MultiByteToWideChar
string WChar2Ansi(const wchar_t* pwszSrc)
{
    int nLen = WideCharToMultiByte(CP_ACP, 0, pwszSrc, -1, NULL, 0, NULL, NULL);
    if (nLen<= 0) return string("");
    char* pszDst = new char[nLen];
    if (NULL == pszDst) return string("");
    WideCharToMultiByte(CP_ACP, 0, pwszSrc, -1, pszDst, nLen, NULL, NULL);
    pszDst[nLen -1] = 0;
    string strTemp(pszDst);
    delete [] pszDst;
    return strTemp;
}
wstring Ansi2WChar(const char* pszSrc, int nLen)
{
    int nSize = MultiByteToWideChar(CP_ACP, 0, pszSrc, nLen, 0, 0);
    if(nSize <= 0) return NULL;
    wchar_t *pwszDst = new wchar_t[nSize+1];
    if( NULL == pwszDst) return NULL;
    MultiByteToWideChar(CP_ACP, 0, pszSrc, nLen, pwszDst, nSize);
    pwszDst[nSize] = 0;
    if( pwszDst[0] == 0xFEFF)                    // skip Oxfeff
        for(int i = 0; i < nSize; i ++) 
            pwszDst[i] = pwszDst[i+1]; 
    wstring wcharString(pwszDst);
    delete pwszDst;
    return wcharString;
}
// Use wcstombs_s and mbstowcs_s 
string ws2s(wstring ws)
{
    const wchar_t* Source = ws.c_str();
    size_t size = 2 * ws.size() + 1;
    char* Dest = new char[size];
    memset(Dest,0, size);
    size_t len = 0;
    wcstombs(Dest, Source, size);
    //wcstombs_s(&len, Dest, size, Source, size);
    string result = Dest;
    delete [] Dest;
    return result;
}
wstring s2ws(string s)
{
    const char* Source = s.c_str();
    size_t size = s.size() + 1;
    wchar_t* Dest = new wchar_t[size];
    wmemset(Dest, 0, size);
    size_t len = 0;
    mbstowcs(Dest, Source, size);
    //mbstowcs_s(&len, Dest, size, Source, size);
    wstring result = Dest;
    delete [] Dest;
    return result;
}
// Testing 
    __int64 elapsed;
    string input = "Hello, World.\\XXX\\XXX";
    wstring winput = L"Hello, World.\\XXX\\XXX";
    wchar_t dest[1000];
    int count = 10000;
    elapsed = time_call([input, &count] 
    {
        while (count)
        {
            Ansi2WChar(input.c_str(), input.length());
            count--;
        }
    });
    memset(dest, 0, 1000);
    _itow_s(elapsed, dest, 10);
    OutputDebugStringW(L"Time:");
    OutputDebugStringW(dest);
    OutputDebugStringW(L"ms\n");
    count = 10000;
    elapsed = 0;
    elapsed = time_call([input, &count] 
    {
        while (count)
        {
            s2ws(input);
            count--;
        }
    });
    memset(dest, 0, 1000);
    _itow_s(elapsed, dest, 10);
    OutputDebugStringW(L"Time:");
    OutputDebugStringW(dest);
    OutputDebugStringW(L"ms\n");
    count = 10000;
    elapsed = 0;
    elapsed = time_call([winput, &count] 
    {
        while (count)
        {
            WChar2Ansi(winput.c_str());
            count--;
        }
    });
    memset(dest, 0, 1000);
    _itow_s(elapsed, dest, 10);
    OutputDebugStringW(L"Time:");
    OutputDebugStringW(dest);
    OutputDebugStringW(L"ms\n");
    count = 10000;
    elapsed = 0;
    elapsed = time_call([winput, &count] 
    {
        while (count)
        {
            ws2s(winput);
            count--;
        }
    });
    memset(dest, 0, 1000);
    _itow_s(elapsed, dest, 10);
    OutputDebugStringW(L"Time:");
    OutputDebugStringW(dest);
    OutputDebugStringW(L"ms\n");
  输出：
  Time:78ms
  Time:94ms
  Time:62ms
  Time:109ms
  从输出结果可以看出
   WideCharToMultiByte
   和
   MultiByteToWideChar
   转换效率要比
   mbstowcs
   和
   wcstombs
   高。
   注意：如果使用
   mbstowcs
   则需要
   Disable
 Specific Warnings： 4996
