---
title: "开源开发者指南：欧盟《人工智能法案》解读"
thumbnail: /blog/assets/189_eu-ai-act-for-oss-developers/thumbnail.png
authors:
- user: brunatrevelin
- user: frimelle
- user: yjernite
translator:
- user: AdinaY
---

# 开源开发者指南：欧盟《人工智能法案》解读

<div style="text-align: center;">
    <b>非法律建议。</b>
</div>

*欧盟《人工智能法案》（EU AI Act）是全球首部全面的人工智能立法，现已正式生效，它将影响我们开发和使用人工智能的方式——包括在开源社区中的实践。如果您是一位开源开发者，正在适应这一新环境，可能会想知道这对您的项目意味着什么。本指南重点解读了该法规的关键要点，特别是针对开源开发者，提供了对这一法规的清晰介绍，并指引您使用相关工具来帮助合规。*

**_免责声明：本指南提供的信息仅供参考，不能被视为任何形式的法律建议。_**

> **快速摘要（TL;DR）：** 《人工智能法案》可能适用于开源 AI 系统和模型，具体规则取决于模型的类型及其发布方式。在大多数情况下，义务包括提供清晰的文档、为部署时披露模型信息添加工具，并遵守现有的版权和隐私规则。幸运的是，许多这些实践已经是开源领域的常见做法。Hugging Face 提供了一些工具来帮助您准备合规，包括支持退出流程、个人数据删除的工具以及文档和许可管理工具。
查看 [模型卡](https://huggingface.co/docs/hub/en/model-cards)、[数据集卡](https://huggingface.co/docs/hub/en/datasets-cards)、[Gradio](https://www.gradio.app/docs/gradio/video)、[水印工具](https://huggingface.co/spaces/meg/watermark_demo)、[支持](https://techcrunch.com/2023/05/03/spawning-lays-out-its-plans-for-letting-creators-opt-out-of-generative-ai-training/)退出机制的[工具](https://huggingface.co/spaces/bigcode/in-the-stack)、[个人数据删除](https://huggingface.co/blog/presidio-pii-detection)、[许可](https://huggingface.co/docs/hub/en/repositories-licenses)等！

欧盟《人工智能法案》是一项具有法律约束力的法规，旨在推动负责任的人工智能发展。为此，它根据人工智能系统或模型可能带来的风险等级设定了相应的规则，同时致力于保护开放研究并支持中小型企业 (SMEs)。如果你是开源开发者，许多工作可能不会受到直接影响，特别是如果你已经在记录系统和跟踪数据来源的话。总体而言，你可以采取一些简单的步骤来做好合规准备。

这项法规将在未来两年内全面生效，适用范围广泛，不仅限于欧盟内的个人或机构。如果你是欧盟以外的开源开发者，但你的人工智能系统或模型在欧盟范围内提供或对欧盟用户产生影响，那么它们也会受到这项法案的约束。

## 🤗 **适用范围**

该法规适用于人工智能技术栈的不同层级，这意味着如果你是供应方（包括开发者）、部署方、分发方等，或者你从事的是人工智能模型或系统的工作，将会有不同的义务。

| **模型**：仅**通用人工智能（GPAI）**模型直接受到法规监管。GPAI 模型是使用海量数据训练的模型，具有显著的通用性，可以执行广泛的任务，并可用于系统和应用程序。例如，大型语言模型（LLM）就是一个例子。对模型的修改或微调也需要符合相关义务。 | **系统**：能够基于输入进行推理的系统。这通常以传统的软件技术栈形式出现，利用或连接一个或多个人工智能模型来处理输入的数字表示。例如，与终端用户交互的聊天机器人，它利用 LLM 或托管在 Hugging Face Spaces 上的 Gradio 应用。 |
|---|---|

在《人工智能法案》中，规则根据人工智能系统或模型可能带来的风险等级进行调整。对于所有人工智能系统，风险可能包括以下几种：

* **[不可接受](https://artificialintelligenceact.eu/article/5/)**：违反人权的系统，例如从互联网上抓取面部图像或使用闭路电视画面的人工智能系统。这类系统被禁止，不能进入市场。
* **[高风险](https://artificialintelligenceact.eu/article/6/)**：可能对人们的安全或基本权利产生不利影响的系统，例如处理关键基础设施、基本服务或执法的系统。这类系统需要在进入市场之前遵循严格的合规步骤。
* **[有限风险](https://artificialintelligenceact.eu/article/50/)**：直接与人互动并可能导致冒充、操控或欺骗风险的系统。这类系统需要满足透明性要求。大多数生成式人工智能模型可以集成到属于这一类别的系统中。作为模型开发者，如果你已经遵守相关要求，例如提供充分的文档，你的模型将更容易被集成到人工智能系统中。
* **最小风险**：大多数系统——即不构成上述风险的系统。它们只需遵守现有法律法规，《人工智能法案》未增加额外要求。

对于**通用人工智能（GPAI）**模型，还存在另一个风险类别，称为**[系统性风险](https://artificialintelligenceact.eu/article/51/)**：即使用大量计算能力的 GPAI 模型，目前定义为训练时计算能力超过 10^25 FLOPs，或者具有高影响能力的模型。根据[斯坦福大学的一项研究](https://crfm.stanford.edu/2024/08/01/eu-ai-act.html)，截至 2024 年 8 月，根据 Epoch 的估算，只有八个模型（Gemini 1.0 Ultra、Llama 3.1-405B、GPT-4、Mistral Large、Nemotron-4 340B、MegaScale、Inflection-2、Inflection-2.5）来自七个开发者（Google、Meta、OpenAI、Mistral、NVIDIA、字节跳动、Inflection）符合系统性风险的默认标准，即其训练至少使用了 10^25 FLOPs。模型是否开源会影响其义务的具体内容。

## 🤗 **如何为合规做好准备**

本指南的**重点**是针对**有限风险人工智能系统和非系统性风险的开源通用人工智能（GPAI）模型**，这些内容应该涵盖了 Hugging Face 平台上大部分公开可用的资源。对于其他风险类别，请务必查阅可能适用的其他义务。

### **针对有限风险人工智能系统**

有限风险的人工智能系统直接与人（终端用户）交互，可能带来冒充、操控或欺骗的风险。例如，生成文本的聊天机器人或文本到图像生成器——这些工具也可能被用来生成误导性内容或深度伪造（deepfake）。《人工智能法案》的目标是通过帮助普通终端用户了解他们正在与人工智能系统交互来应对这些风险。目前，大多数通用人工智能模型尚不被视为具有系统性风险。在有限风险人工智能系统的情况下，无论它们是否开源，都需要遵守以下义务：

**有限风险人工智能系统的开发者需要：**
- 向用户披露他们正在与人工智能系统交互，除非这一点显而易见。需要注意，终端用户可能没有与技术专家相同的技术理解，因此你需要以清晰且详尽的方式提供这一信息。
- 标记合成内容：人工智能生成的内容（如音频、图像、视频、文本）必须明确标记为人工生成或人工操控，并以机器可读的格式提供。现有的工具（例如 Gradio 的[内置水印功能](https://huggingface.co/spaces/meg/watermark_demo)）可以帮助你满足这些要求。

**需要注意的是，你不仅可能是人工智能系统的“开发者”，还可能是“部署者”。**  

人工智能系统的部署者是指在其专业领域中使用人工智能系统的人或公司。在这种情况下，还需要遵守以下义务：

- **对于情绪识别和生物特征识别系统：** 部署者必须告知个人这些系统的使用情况，并按照相关法规处理个人数据。
- **披露深度伪造和人工智能生成的内容：** 部署者必须披露何时使用了人工智能生成的内容。如果内容属于艺术作品的一部分，则需以不破坏用户体验的方式披露内容是生成或操控的。

以上信息需要用清晰的语言提供，并且最迟需要在用户首次与人工智能系统交互或接触时披露。

人工智能办公室（AI Office）负责执行《人工智能法案》，将协助制定实践准则，提供检测和标记人工生成内容的指导。这些准则目前正在由行业和民间社会参与撰写，预计将于2025年5月发布。相关条款将从2026年8月开始实施。

### **针对非系统性风险的开源通用人工智能（GPAI）模型**

如果你正在开发非系统性风险的开源通用人工智能（GPAI）模型，例如大型语言模型（LLM），以下条款将适用。根据《人工智能法案》，**开源**是指“在免费和开源许可下发布的软件和数据，包括模型，可以自由共享，并允许用户自由访问、使用、修改和再分发它们或其修改版本”。开发者可以从 Hugging Face 平台上列出的[开源许可证](https://huggingface.co/docs/hub/en/repositories-licenses)中选择适合的许可证，并检查所选许可证是否符合[《人工智能法案》的开源定义](https://ai-act-law.eu/recital/102/)。

**对于非系统性风险的开源 GPAI 模型的义务如下：**

- **撰写并提供充分详细的训练内容摘要**：  
  根据人工智能办公室（AI Office）提供的模板，开发者需要撰写并公开一份足够详细的总结，描述用于训练 GPAI 模型的内容。  
  - 训练内容的细节级别尚在讨论中，但应相对全面。  

- **实施一项符合欧盟版权法及相关权利的政策，特别是针对选择退出（opt-out）的合规政策**：  
  开发者需要确保他们被授权使用受版权保护的材料，这可以通过权利人的授权获得，或在适用版权例外和限制的情况下使用。其中一个例外是“文本与数据挖掘（TDM）”例外，这项技术广泛用于检索和分析内容。然而，当权利人明确表示保留其作品用于这些目的的权利时（即选择退出），TDM 例外通常不适用。  
  在制定符合欧盟《版权指令》的政策时，这些选择退出应被尊重，并限制或禁止使用受保护的材料。换句话说，只要尊重作者选择退出人工智能训练的决定，基于版权材料的训练并不违法。  
  - 关于选择退出应如何以技术方式表达（尤其是机器可读的格式），仍有一些未决问题。然而，尊重网站 robots.txt 文件中表达的信息并使用类似 [Spawning](https://spawning.ai/) 提供的 API 是一个良好的起点。

**《人工智能法案》还与现有的版权和个人数据法规（如[版权指令](https://eur-lex.europa.eu/eli/dir/2019/790/oj)和[数据保护法规](https://eur-lex.europa.eu/eli/reg/2016/679/oj)）挂钩。**  
在这方面，你可以参考 Hugging Face 集成的工具，例如[支持](https://techcrunch.com/2023/05/03/spawning-lays-out-its-plans-for-letting-creators-opt-out-of-generative-ai-training/)改进[选择退出](https://huggingface.co/spaces/bigcode/in-the-stack)机制的工具，以及[个人数据删除](https://huggingface.co/blog/presidio-pii-detection)工具，同时关注欧洲和国家机构（如[法国数据保护局 CNIL](https://www.cnil.fr/fr/ai-how-to-sheets)）的建议。

**在 Hugging Face 平台上的项目中，已有实现选择退出训练数据机制的案例**，例如 BigCode 团队的 [Am I In The Stack](https://huggingface.co/spaces/bigcode/in-the-stack) 应用，以及集成了 [Spawning 小部件](https://techcrunch.com/2023/05/03/spawning-lays-out-its-plans-for-letting-creators-opt-out-of-generative-ai-training/)以处理包含图像 URL 的数据集。通过这些工具，创作者可以简单地选择退出，拒绝其受版权保护的材料被用于人工智能训练。随着选择退出流程的不断开发，这些工具能有效帮助创作者公开表达他们不希望其内容用于人工智能训练的决定。

开发者可以依赖实践准则（目前正在制定中，预计将于 2025 年 5 月发布）来证明其已履行这些义务。

如果你的工作方式不符合《人工智能法案》中对**开源**的定义，则需要遵守其他义务。

此外，请注意，如果某个 GPAI 模型满足系统性风险的条件，其开发者必须通知欧盟委员会。在通知过程中，开发者可以根据模型的具体特性论证其模型不会构成系统性风险。委员会将根据提交的论据以及模型的具体特性和能力审查每项论证，并决定接受或拒绝。如果委员会拒绝开发者的论证，该 GPAI 模型将被指定为构成系统性风险，并需要遵守进一步的义务，例如提供有关模型的技术文档，包括其训练和测试过程及其评估结果。

**GPAI 模型的义务将从 2025 年 8 月开始执行。**

## 🤗 **参与其中**

欧盟《人工智能法案》的许多实际应用内容仍在通过公共咨询和工作组的形式进行开发，其结果将决定如何使法案的条款更加便于中小企业（SMEs）和研究人员实现合规。如果你对这些过程的具体实施有兴趣，现在是一个绝佳的时机参与其中，帮助塑造这一进程！

```
@misc{eu_ai_act_for_oss_developers,
  author    = {Bruna Trevelin and Lucie-Aimée Kaffee and Yacine Jernite},
  title     = {Open Source Developers Guide to the EU AI Act},
  booktitle = {Hugging Face Blog},
  year      = {2024},
  url       = {},
  doi       = {}
}
```

*感谢 Anna Tordjmann、Brigitte Tousignant、Chun Te Lee、Irene Solaiman、Clémentine Fourrier、Ann Huang、Benjamin Burtenshaw 和 Florent Daudens 提供的反馈、评论和建议。*