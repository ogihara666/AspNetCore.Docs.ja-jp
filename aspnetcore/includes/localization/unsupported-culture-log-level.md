> [!NOTE]
> <span data-ttu-id="a849a-101">ASP.NET Core 3.0 より前の Web アプリでは、要求されたカルチャがサポートされていない場合、要求ごとに `LogLevel.Warning` の種類のログが 1 つ書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="a849a-101">Prior to ASP.NET Core 3.0 web apps write one log of type `LogLevel.Warning` per request if the requested culture is unsupported.</span></span> <span data-ttu-id="a849a-102">要求ごとに 1 つの `LogLevel.Warning` をログに記録する場合、冗長な情報を含む大きなログ ファイルが作成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a849a-102">Logging one `LogLevel.Warning` per request is can make large log files with redundant information.</span></span> <span data-ttu-id="a849a-103">この動作は、ASP.NET 3.0 で変更されています。</span><span class="sxs-lookup"><span data-stu-id="a849a-103">This behavior has been changed in ASP.NET 3.0.</span></span> <span data-ttu-id="a849a-104">`RequestLocalizationMiddleware` によって `LogLevel.Debug` の種類のログが書き込まれるため、運用ログのサイズが縮小されます。</span><span class="sxs-lookup"><span data-stu-id="a849a-104">The `RequestLocalizationMiddleware` writes a log of type `LogLevel.Debug`, which reduces the size of production logs.</span></span>