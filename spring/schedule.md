# @Schedule

스케줄링을 사용하면 자바스크립트의 setTimeout, interval 처럼 코드에 딜레이를 주거나 주기적으로 실행 시키는 것이 가능하다.
예를 들어 지속적으로 데이터를 쌓거나 분석하는 경우에 사용하기도 하고 일정 시간이 지나면 만료 시키는 등에 활용 가능하다.

```
@Scheduled(fixedRateString = "60000")   // 1분 마다 실행
public void scheduledTask() {
  ...
}
```

동적으로 스케줄링을 하고 제거하는 코드도 가능하다.

```
private Map<String, ScheduledFuture<?>> scheduledTasks = new ConcurrentHashMap<>();

/**
 * 스케줄 등록 
 * @param token 
 * @param expiresIn
 */
 public void register(String token, Long expiresIn) {
    tokenExpireCountMap.put(token, 0);     
    ScheduledFuture<?> tokenExpireTask = taskScheduler.scheduleWithFixedDelay(
            () -> {               
            if (tokenExpireCountMap.get(token) == 1) {
                        userService.logout(token);                 
                        this.remove("loginUser"+token);
            }                
            tokenExpireCountMap.put(token, tokenExpireCountMap.get(token)+1);
        }, expiresIn * 1000);   
        scheduledTasks.put("loginUser"+token, tokenExpireTask);
    }
    
/** 
  * 스케줄 삭제 
  * @param id 
  */
  public void remove(String id) {    
      scheduledTasks.get(id).cancel(true);
  }
```

[Spring Schedule](https://jeong-pro.tistory.com/186)
