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

