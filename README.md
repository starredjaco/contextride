# ContextRide

## Abstract - EN

Modern Endpoint Detection and Response (EDR) and Extended Detection and Response (XDR) solutions have evolved far beyond simple function interception in `ntdll.dll`. The current detection surface encompasses static scanning of PEs on disk write, call stack analysis to identify anomalous syscalls, Protected Process Light (PPL) over critical processes, and behavioral telemetry via ETW. These layers have rendered classic evasion techniques — direct syscalls, indirect syscalls with custom stubs, and use of `MiniDumpWriteDump` — progressively ineffective against any modern security product.

This paper introduces ContextRide, a technique that uses `NtContinue` as an indirect syscall execution primitive, preserving a call stack with legitimate frames from `ntdll.dll` and `kernelbase.dll` without introducing detectable stubs outside `ntdll.dll` in memory. Unlike approaches such as SysWhispers2/3 — which produce detectable anomalous call stacks — and extending beyond HookChain, which achieves call stack legitimacy via IAT patching but does not address static scanning, in-memory dump format, or PPL, ContextRide evades call stack inspection based on frame address verification without fabricating synthetic frames, simultaneously bypassing hook interception, static scanning, and, operationally, PPL.

Complementary techniques complete the toolchain: a custom Minidump builder that avoids `MiniDumpWriteDump`; in-memory construction of the MDMP format with obfuscation of sensitive constants via `volatile` (effective against byte-exact YARA matching rules); semantic binary padding to bypass position-sensitive YARA rules; and separation of detectable MDMP structure writes for post-processing in Python outside the target host. The complete toolchain was validated against 15 EDR/XDR products on Windows 11 builds 26100 and 26200, producing a functional `lsass.exe` dump without triggering any detection layer in 5 of the 8 products tested to date, with tests ongoing for the remaining 7.

## Abstract - PT-BR

Soluções modernas de Endpoint Detection and Response (EDR) e Extended Detection and Response (XDR) evoluíram muito além da simples interceptação de funções na `ntdll.dll`. A superfície de detecção atual abrange scanning estático de PEs na escrita em disco, análise de call stack para identificar syscalls anômalos, Protected Process Light (PPL) sobre processos críticos e telemetria comportamental via ETW. Essas camadas tornaram técnicas clássicas de evasão — direct syscalls, indirect syscalls com stubs próprios e uso de `MiniDumpWriteDump` — progressivamente ineficazes contra qualquer produto de segurança moderno.

Este artigo introduz o ContextRide, uma técnica que utiliza `NtContinue` como primitiva de execução de syscalls indiretos, preservando um call stack com frames legítimos de `ntdll.dll` e `kernelbase.dll` sem introduzir stubs detectáveis fora da `ntdll.dll` em memória. Ao contrário de abordagens como SysWhispers2/3 — que produzem call stacks anômalos detectáveis — e estendendo-se além do HookChain, que alcança legitimidade de call stack via IAT patching mas não endereça scanning estático, formato de dump em memória nem PPL, o ContextRide evade a inspeção de call stack baseada em verificação de endereços de frame sem fabricar frames sintéticos, contornando simultaneamente a interceptação por hook, o scanning estático e, operacionalmente, o PPL.

Técnicas complementares completam o toolchain: um builder customizado de Minidump que evita `MiniDumpWriteDump`; construção in-memory do formato MDMP com ofuscação de constantes sensíveis via `volatile` (eficaz contra regras YARA de correspondência byte-exata); padding semântico binário para contornar regras YARA position-sensitive; e separação da escrita de estruturas MDMP detectáveis para pós-processamento em Python fora do host alvo. O toolchain completo foi validado contra 15 produtos EDR/XDR nos builds 26100 e 26200 do Windows 11, produzindo um dump funcional de `lsass.exe` sem acionar nenhuma camada de detecção em 5 dos 8 produtos testados até o momento, com testes em andamento nos demais 7.

## ACM Reference Format

```
Danilo Albuquerque. 2026. ContextRide: NtContinue as an Execution Primitive for EDR/XDR Evasion in Userland. Brazil. https://github.com/0xn4d/contextride
```

## White paper

- [English](https://github.com/0xn4d/contextride/blob/master/ContextRide%20-%20NtContinue%20as%20an%20Execution%20Primitive%20for%20EDR%20or%20XDR%20Evasion%20in%20Userland.pdf)
- [Português](https://github.com/0xn4d/contextride/blob/master/ContextRide%20-%20NtContinue%20como%20Primitiva%20de%20Execucao%20para%20Evasao%20de%20EDR%20ou%20XDR%20em%20Userland.pdf)

## Contact

- LinkedIn contact: https://www.linkedin.com/in/daniloalbuqrque/
- GitHub: https://github.com/0xn4d

## My public releases regarding ContextRide:

- [Aurora EDR bypass](https://github.com/0xn4d/contextride/tree/master/evidences)
- [CrowdStrike Falcon bypass](https://github.com/0xn4d/contextride/tree/master/evidences)
- [Elastic Defend bypass](https://github.com/0xn4d/contextride/tree/master/evidences)
- [Microsoft Defender bypass](https://github.com/0xn4d/contextride/tree/master/evidences)
- [MalwareBytes bypass](https://github.com/0xn4d/contextride/tree/master/evidences)
