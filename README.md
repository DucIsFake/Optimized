# DucIsHere – HyperWorldGen (Paged + LOD + Chunk Preview)

**Author:** DucIsHere  
**Team:** Wandering Developer  
**Minecraft Version:** 1.20.x+  
**Loaders:** Fabric / Quilt / Forge  

HyperWorldGen là mod world generation **siêu khổng lồ**, kết hợp:  

- **Paged Worldgen**: chỉ gen chunk gần player ±1000 block để tối ưu RAM/CPU  
- **LOD 3D**: render xa theo mesh / heightmap, giảm block detail khi không cần thiết  
- **Chunky-style preview**: review toàn cảnh world mà không chiếm full RAM  
- **Surface / Cave gen**: gen block theo trục Y ± (có thể gen cao 20k+ hoặc sâu âm)  

---

## ⚡ Features

- Gen chunk **theo radius** xung quanh player, từng chunk 1  
- Gen surface/cave hoặc **block theo Y ±** (0 → yHeight hoặc 0 → yHeight âm)  
- Chia chunk thành **section 16 block** để giảm lag spike  
- **Async gen + progress tracking + cancel**  
- Hỗ trợ **LOD 3D**, phù hợp chơi survival và review cinematic  
- Compatible với **4500+ biome, End, Nether, Overworld**  

---

## 🛠 Commands

### 1. Gen chunk
```text
/chunk radius <R> gen y=<Y>
```
R = số chunk xung quanh player
Y > 0 → gen surface (0 → Y)
Y < 0 → gen cave (0 → Y âm)
Gen từng chunk 1, mỗi chunk chia section 16 block

### 2. Progress
```text
/progress chunk
```
Hiển thị số chunk đã gen / tổng chunk trong queue
Monitor khi gen chunk radius lớn hoặc Y cao

### 3. Cancel
```text
/chunk cancel
```
Dừng gen chunk giữa chừng
Clear queue + thread async
Báo số chunk đã gen

---

🧩 LOD 3D
-Level 0: block thật, gần player (bong bóng ±1000 block)

-Level 1: mesh, trung bình, xa player

-Level 2: heightmap / biome color, cực xa player

-Hỗ trợ X/Z/Y → render toàn cảnh world khổng lồ mà vẫn tối ưu RAM

---

💡 Usage Tips
-Gen Y cực cao (ví dụ 20k) → nên gen radius nhỏ, bật async + sleep giữa chunk/section để tránh lag

-Khi review world → sử dụng LOD mesh, không cần block detail full

-Surface/Cave logic → hỗ trợ y > 0 / y < 0 cho phép gen cả End/Nether/Overworld

---

🔧 Notes
-Chunk được chia section 16 block → từng chunk 1 → giảm lag

-Thread async gen → có thể kết hợp multi-thread / batch gen để tăng tốc

-Hoàn toàn tương thích Paged Worldgen runtime + Chunky-style review + Distant Horizons LOD

---

🔹 ASCII Flow minh họa pipeline
```mathematica
Player Command
   │
   ├─ /chunk radius <R> gen y=<Y> ──► ChunkPos Queue
   │                                   │
   │                                   ▼
   │                         ChunkGenManager (Async Thread)
   │                                   │
   │                          ┌────────┴────────┐
   │                          │                 │
   │                          ▼                 ▼
   │                  genChunkYRange          Progress Update
   │                  (section 16 block)       │
   │                          │                 │
   │                          ▼                 │
   │                  genSection(pos, yBase)   │
   │                  (noise + biome + block) │
   │                          │                 │
   │                          ▼                 │
   │                  Update Chunk Cache       │
   │                          │                 │
   │                          ▼                 │
   │                  Apply LOD 3D Mesh       │
   │                 (far chunk → heightmap)  │
   │                          │
   └─ /progress chunk ─────────┘
        └─ Show chunksGen / totalChunks

Player Action /chunk cancel
   └─ ChunkGenManager.isRunning = false
       └─ Clear Queue → stop thread → progress báo số chunk đã gen
```

