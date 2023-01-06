### Typescript在Cocos Creator中的代码规范

### 1. 不应过多地使用回调，尽量使用异步的方式

```typescript
// Bad
resources.loadDir("/textures/cells", SpriteFrame, function (err, assets) {
	....
  resources.load("/json/level", JsonAsset, function (err, asset) {
    ....
    resources.loadDir("/prefabs/cells", function (err, assets) {
      if (err) {
        console.error(err);
        return;
      }
      ....
    })
  })
})

// Good
// 加载图片资源
const spriteFrames = await ResourceCacheManager.Instance.syncLoadDir<SpriteFrame>("/textures/cells");
if (!spriteFrames) return;

// 加载预制资源
const prefabs = await ResourceCacheManager.Instance.syncLoadDir<Prefab>("/prefabs/cells");
if (!prefabs) return;

// 加载关卡数据
const missionJson = await ResourceCacheManager.Instance.syncLoadFile<JsonAsset>("/json/level");
if (!missionJson) return;
```

