# ILSpy

## Malware Analysis & .NET Decompilation Reference

ILSpy is an open-source, free .NET assembly browser and decompiler that reconstructs compiled .dll, .exe, and .winmd files back into structured C# code.



<img width="949" height="463" alt="image" src="https://github.com/user-attachments/assets/99a4ccf5-08e0-4459-823a-2fd559fbf201" />


 
Make sure to open it in virtualmachine, disable network connection and make a snapshot to work with and later on you can clean up by restoring.

Assembly Tree Panel (left)
•	Expand a node to see Namespaces → Classes → Methods / Fields / Properties
•	Right-click any node for context menu actions (Analyze, Go To, Copy, etc.)
•	Use the search bar (Ctrl+F in the tree) to jump to a type or member by name

Decompiled Code Panel (right)
•	Displays C# (default), IL, or ReadyToRun disassembly depending on the Language drop-down
•	Click any identifier to highlight all usages in the current view
•	Ctrl+Click an identifier to navigate to its definition
The Analyze Feature — Follow the Data

Analyze is your best friend for tracing how variables, fields, and methods are used across the entire assembly. Right-click ANY member in the tree or in the code view, then click Analyze.

What Analyze Shows
Shortcut / Action	What It Does
Used By	All callers / sites that READ or REFERENCE this member
Uses	All members this item calls or references internally
Instantiated By	Who creates instances of this class (for types)
Overridden By	Sub-classes that override this method
Exposed By	Properties / fields that expose this type

Analyze is your best friend for tracing how variables, fields, and methods are used across the entire assembly. Right-click ANY member in the tree or in the code view, then click Analyze.

What Analyze Shows
Shortcut / Action	What It Does
Used By	All callers / sites that READ or REFERENCE this member
Uses	All members this item calls or references internally
Instantiated By	Who creates instances of this class (for types)
Overridden By	Sub-classes that override this method
Exposed By	Properties / fields that expose this type

Pin the Analyze panel (icon top-right of the panel) so it stays visible while you navigate. Otherwise it closes when you click away.



<img width="727" height="360" alt="image" src="https://github.com/user-attachments/assets/8bd62834-3e80-4848-ac6f-fa07de67caee" />




it will let u search by that specific assembly 




<img width="1112" height="160" alt="image" src="https://github.com/user-attachments/assets/a590df57-5876-4d46-b703-d68d50c15c2a" />


- High-Value Search Terms for Malware
Shortcut / Action	What It Does
WebClient	HTTP download / C2 beaconing
TcpClient / Socket	Raw TCP connections, reverse shells
Process.Start	Spawning child processes
Registry	Persistence via Run keys
Assembly.Load	Reflective loading / in-memory execution
Convert.FromBase64	Encoded payload decoding
Environment.GetFolder	Temp, AppData path construction
Mutex	Single-instance checks (malware anti-re-run)
Thread / Timer	Background tasks, sleep for C2 beacon intervals
Marshal / unsafe	Shellcode injection, P/Invoke abuse
DllImport	Native API calls — look for VirtualAlloc, WriteProcessMemory


There is different scopes to dive into, which imports, stick to C2 scope for the type of connections are being made…etc etc
Here for example we it Serialize the C2 packet into data structure.



<img width="1052" height="207" alt="image" src="https://github.com/user-attachments/assets/d2be0094-eb36-4eae-93d3-72c2a4255638" />



As you keep tracing for More IOC you can find more for example which in this example a keylogger



<img width="949" height="184" alt="image" src="https://github.com/user-attachments/assets/186fd220-de23-4238-89a6-e8b07fd04b86" />




<img width="949" height="260" alt="image" src="https://github.com/user-attachments/assets/0383de46-0a33-48fe-972e-2b37949ee421" />



Please the tools for Deobfuscation I have found, you can set your Analysis Env for these and save these new snapshot.  (IK IK i haven't used even half but getting my hands
on multiple ones at least helps me with this garbage)


  **Tier 1 — Run These First, Always**
de4dot	Renames obfuscated members, decrypts strings for known obfuscators	github.com/de4dot/de4dot
Detect-It-Easy (DIE)	Identifies the exact packer/obfuscator/compiler before you waste time	github.com/horsicq/Detect-It-Easy
CFF Explorer	PE header inspection, .NET metadata viewer, resource extractor	ntcore.com



  **Tier 2 — .NET Specific Deobfuscation**
NETReactorSlayer	Specifically destroys .NET Reactor protection — very effective
ConfuserEx-Unpacker	Multiple forks exist for ConfuserEx variants
de-dotfuscator	Handles Dotfuscator-specific string encryption
ViRb3/de4dot-cex	Updated de4dot fork, handles newer ConfuserEx builds
MindLated	Handles KoiVM (virtualization-based obfuscator) — rare but powerful
EazFixer	Specifically targets Eazfuscator — string/resource decryption
InvisibleNet Unpacker	For .NET samples packed with InvisibleNet


  **Tier 3 — Dynamic Dumping (when static fails)**
Tool	What it does
ExtremeDumper	Dumps .NET assemblies from memory at runtime — catches unpackers mid-execution
MegaDumper	Similar to ExtremeDumper, different hook approach
hollows_hunter	Detects and dumps hollowed/injected processes
ProcessHacker / System Informer	Watch .NET assemblies loading live in memory
dnSpyEx	Fork of dnSpy — has a built-in debugger, set breakpoints, inspect values at runtime



  **Tier 4 — String Decryption & Control Flow**
OldRod	KoiVM devirtualizer — reconstructs real IL from VM bytecode
NetUnpack	Generic .NET unpacker for runtime-packed assemblies
il-repack	Merges multi-assembly obfuscated packages back into one
Subtractor	Control flow deobfuscation — fixes opaque predicates
CodeCracker	Handles some SmartAssembly string decryption automatically



  **Tier 5 — Analysis Companions**
dnSpy / dnSpyEx	Alternative to ILSpy — better debugger, edit-and-continue
dotPeek (JetBrains)	Another decompiler — sometimes succeeds where ILSpy fails
JustDecompile (Telerik)	Free, sometimes handles edge cases differently
ILDasm	Microsoft's own IL disassembler — ships with .NET SDK, ground truth
PEview / PE-bear	Raw PE structure — find embedded payloads in sections
Wireshark	Capture the actual C2 traffic — now you know it's protobuf, you can parse it



  **Tier 6 — For Your Specific Sample (PUA Unicode)**
dnSpyEx debugger	Set a breakpoint on Serializer.Serialize — inspect packet at runtime to see actual C2 data
Fakenet-NG	Fake network services — let the malware "connect" and capture what it sends
WireShark + protobuf decoder	Capture traffic, decode protobuf bytes into readable fields





