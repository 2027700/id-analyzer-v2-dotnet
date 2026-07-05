李冬
[![NuGet版本](https://img.shields.io/nuget/v/IDAnalyzerV2.svg)](https://www.nuget.org/packages/IDAnalyzerV2)
[![Nu Get下载](https://img.shields.io/nuget/dt/IDAnalyzerV2.svg)](https://www.nuget.org/packages/IDAnalyzerV2)
[![许可证](https://img.shields.io/badge/license-MIT-blue.svg)](许可证)

官方的.NET/C#客户端库**[身份证（ID）分析器](https://www.idanalyzer.com)A pi v2**–在几分钟内实现身份证件验证、KYC入职和生物识别检查的自动化。

扫描并验证**护照、驾照、身份证、签证和居留许可，来自190多个国家**，跑步**1:1面部匹配和活力检测**，屏幕对准**反洗钱/PEP/制裁**监视列表和远程运行的机载用户**DocuPass**托管验证和电子签名。

- 🌐 **网址:** [www.idanalyzer.com](https://www.idanalyzer.com)
- 📚 **开发人员文档和应用程序接口参考资料：** [developer.idanalyzer.com](https://developer.idanalyzer.com/help)
- 📖 **完整SDK类引用（自动生成）:** [https://idanalyzer.github.io/id-analyzer-v2-dotnet/](https://idanalyzer.github.io/id-analyzer-v2-dotnet/)
- 🔑 **获取您的API密钥:** [portal2.id分析器.com](https://portal2.idanalyzer.com)
- 💬 **支持:** support@idanalyzer.com

##特点

- **文档OCR与认证**—来自190多个国家的护照、驾照、身份证、签证和居留许可识别，包括MRZ和PDF417/AAMVA条形码解析。
- **生物特征验证**—1:1面部匹配和生气/表现-攻击检测。
- **急性淋巴细胞白血病筛查**—人防、制裁、观察名单和负面媒体检查。
- **DocuPass** — hosted, no-code remote identity verification, KYC/AML onboarding and legally-binding e-signature.
- **KYC profiles, transaction vault, contract generation and webhooks.**
- **US & EU data-residency regions.**

> ⚠️ Never embed your API key in client-side apps (mobile, browser JS). Call the API from your server.

## Installation

The v2 SDK ships as **`IDAnalyzerV2`** (the legacy `IDAnalyzer` package id remains the API v1 SDK):

```bash
dotnet add package IDAnalyzerV2
```

Targets .NET Standard 2.1 (works with .NET Core 3.x, .NET 5/6/8+).

## Authentication & region

Pass your API key to each client, or set the `IDANALYZER_KEY` environment variable. The SDK targets the US endpoint (`https://api2.idanalyzer.com`) by default; set `IDANALYZER_REGION=eu` for the EU endpoint (`https://api2-eu.idanalyzer.com`). An unrecognized region throws `InvalidArgumentException`.

## Quick start

```csharp
using IDAnalyzer;

var scanner = new Scanner("YOUR_API_KEY");
scanner.throwApiException(true);
scanner.setProfile(new Profile(Profile.SECURITY_MEDIUM));

// Scan a document + selfie for biometric verification
var result = scanner.scan("id_front.jpg", "", "selfie.jpg");
Console.WriteLine(result["decision"]);   // accept / review / reject
```

## Examples

```csharp
using IDAnalyzer;

// AML / PEP / sanctions screening
var aml = new AML("YOUR_API_KEY");
aml.search("John Smith", "", 0, "US");        // POST /aml
aml.searchV3("John Smith", "", 10, 1);        // POST /amlv3

// KYB — business verification
// Verify a business from its registration/incorporation document: extract
// details, check official company registries, screen against sanctions/PEP,
// and return directors/owners to verify.
var kyb = new KYB("YOUR_API_KEY");
kyb.verify("registration.jpg");                                       // from a document
kyb.verify("", "ACME CORPORATION", "", "12345678", "", "", "US");     // from known details

// DocuPass — hosted remote verification link
var docupass = new Docupass("YOUR_API_KEY");
var link = docupass.createDocupass("YOUR_PROFILE_ID");
Console.WriteLine(link["url"]);
```

## API coverage

The SDK wraps the complete ID Analyzer API v2 surface:

| Class | Methods |
|---|---|
| `Scanner` | `scan`, `quickScan`, `veryQuickScan` |
| `Biometric` | `verifyFace`, `verifyLiveness` |
| `AML` | `search` (`/aml`), `searchV3` (`/amlv3`) |
| `KYB` | `verify` (`/kyb`) |
| `Contract` | `generate` + template CRUD |
| `Transaction` | `getTransaction`, `listTransaction`, `updateTransaction`, `deleteTransaction`, `exportTransaction`, `saveImage`, `saveFile` |
| `Docupass` | `createDocupass`, `listDocupass`, `getDocupass`, `deleteDocupass` |
| `ProfileAPI` | KYC profile create / list / get / update / delete / export |
| `Webhook` | `listWebhook`, `resendWebhook`, `deleteWebhook` |
| `Account` | `getAccount` |
| `Profile` | client-side KYC profile-override builder |

## Resources

- [ID Analyzer website](https://www.idanalyzer.com)
- [Developer documentation & API reference](https://developer.idanalyzer.com/help)
- [.NET SDK guide](https://developer.idanalyzer.com/help/net)
- [Dashboard — get your API key](https://portal2.idanalyzer.com)

## Other ID Analyzer SDKs

[PHP](https://github.com/idanalyzer/id-analyzer-v2-php) · [Python](https://github.com/idanalyzer/id-analyzer-v2-python) · [Node.js](https://github.com/idanalyzer/id-analyzer-v2-nodejs) · [.NET](https://github.com/idanalyzer/id-analyzer-v2-dotnet) · [Java](https://github.com/idanalyzer/id-analyzer-v2-java) · [Go](https://github.com/idanalyzer/id-analyzer-v2-go)

## License

MIT © [ID Analyzer](https://www.idanalyzer.com) — see [LICENSE](LICENSE).
