# LeetCode 100题

### DAY 08-04

#### 两数之和

![image-20220805000807395](assets/image-20220805000807395.png)

```java

    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                int[] result = new int[2];
                result[0] = map.get(target - nums[i]);
                result[1] = i;
                return result;
            }
            map.put(nums[i], i);
        }
        return null;
    }
```

