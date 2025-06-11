# MISSION 02
## MISSION: 예시 만들기

본인이 잘 이해했는지 확인하는 가장 정확한 방법은 가르쳐 보는 것!
클린코드 읽으며 뼈맞았던 내용 중 **`3가지 원칙`** 을 고르고, 원칙 따르는 예시 총 3가지를 만들어보세요.

클린코드 읽을 때 분명 참고하라고 적어준 예시인데 자바로 되어있어서 공감이 잘 안됐죠? 이제 본인이 가장 잘하는 언어로(JS, Python 등등) 더러운 코드를 깨끗한 코드로 리팩토링하는 예시를 만들어보세요.

### (예시) 원칙 1. 함수는 한 가지를 해야한다.

---

- **(예시) Before 😣**

```jsx
function sendEmailToClient(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);

    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

- **무엇을 고치려고 하는지, 고치려는 문제가 무엇인지 알려주세요.**

클라이언트 찾아서 active하면 이메일을 보내려고 합니다. 지금 코드는 `클라이언트에게 이메일을 보내는 함수` 안에 `데이터베이스에서 active 클라이언트를 찾아서` 가 포함되어 있습니다.

- **(예시) After 😎**

```jsx
function sendEmailToActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}

```

- **어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.**

하나의 함수가 한 가지의 일만 하도록 할일을 나누었습니다.

---

### QUIZ 01.

```jsx
// 본인이 가장 잘하는 언어로(JS, Python 등등) 더러운 코드를 깨끗한 코드로 
// 리팩토링하는 예시를 만들어보세요. 현재 파일은 JS 로 되어있지만. 자유롭게 다른 언어로 변경해주세요. 

// 원칙 1. 함수는 한 가지 일을 해야한다(SRP)

// Before 😣

this.aceCanvasMapSvc.setCanvasObj('#event_reference_canvas_pipe', '/assets/json/pipe_risk.json').then((pipeObj: CanvasObject) => {
  this.acePipeRiskSvc.initializeOrUpdateRiskPipes(pipeObj);
  this.acePipeRiskSvc.drawRiskPipe(pipeObj);
  this.aceCanvasMapSvc.drawMiniMap('#event_reference_canvas_pipe', '#minimap_canvas_line');
});
this.aceCanvasMapSvc.setCanvasObj('#event_reference_canvas_valve', '/assets/json/valve_info.json').then((valveObj: CanvasObject) => {
  this.acePipeRiskSvc.initializeOrUpdateValves(valveObj);
  this.acePipeRiskSvc.drawValve(valveObj);
  this.aceCanvasMapSvc.drawMiniMap('#event_reference_canvas_valve', '#minimap_canvas_valve');
});

// 무엇을 고치려고 하는지, 고치려는 문제가 무엇인지 알려주세요.
// - initEvent 함수 내용의 여러 책임을 분리하고 명확한 함수명으로 구조를 개선해야했습니다.
  

// After 😎

private async drawPipeRiskCanvas(): Promise<void> {
  const pipeObj = await this.aceCanvasMapSvc.setCanvasObj(
    '#event_reference_canvas_pipe',
    '/assets/json/pipe_risk.json'
  );

  this.acePipeRiskSvc.initializeOrUpdateRiskPipes(pipeObj);
  this.acePipeRiskSvc.drawRiskPipe(pipeObj);
  this.aceCanvasMapSvc.drawMiniMap('#event_reference_canvas_pipe', '#minimap_canvas_line');
}

private async drawValveCanvas(): Promise<void> {
  const valveObj = await this.aceCanvasMapSvc.setCanvasObj(
    '#event_reference_canvas_valve',
    '/assets/json/valve_info.json'
  );

  this.acePipeRiskSvc.initializeOrUpdateValves(valveObj);
  this.acePipeRiskSvc.drawValve(valveObj);
  this.aceCanvasMapSvc.drawMiniMap('#event_reference_canvas_valve', '#minimap_canvas_valve');
}
  
// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// - 기존에 합쳐져있던 관로와 밸브 캔버스 관련 로직을 분리했고, 
// - 함수명을 직관적이고 명확하게 이해할 수 있도록 작성했습니다.

```

### QUIZ 02.

```jsx
// 본인이 가장 잘하는 언어로(JS, Python 등등) 더러운 코드를 깨끗한 코드로 
// 리팩토링하는 예시를 만들어보세요. 현재 파일은 JS 로 되어있지만. 자유롭게 다른 언어로 변경해주세요. 

// 원칙 2. DRY - Do not Repeat Yourself: 반복하지 마라

// Before 😣

/**
 * 주어진 날짜 범위를 바탕으로 알람 계측기 목록을 가져와서 지도에 표시합니다.
 * @param startDate 시작 날짜(YYYY-MM-DD)
 * @param endDate 종료 날짜(YYYY-MM-DD)
 */
async updateDate(startDate: string, endDate: string): Promise<void> {
  // 내부적으로 날짜 상태를 저장하고, 관련 이벤트 스트림 발행
  this.startDate = startDate;
  this.endDate = endDate;
  this.$mapDate.next({ startDate, endDate });

  try {
    // 새로운 구조에 맞춰 midZone / subZone 파라미터를 구분하여 설정
    let subZoneParam: string | undefined = undefined;
    let midZoneParam: string | undefined = undefined;

    if (this.selectedZoneType === 'sub' && this.selectedSubZones.length > 0) {
      // 소구역 모드이며, 선택된 소구역이 있는 경우
      // 배열을 콤마로 join하여 요청 파라미터를 구성
      subZoneParam = this.selectedSubZones.join(',');
    } else if (this.selectedZoneType === 'mid' && this.selectedMidZone) {
      // 중구역 모드이며, 중구역이 선택된 경우
      midZoneParam = this.selectedMidZone;
    } else {
      // 아무 구역도 선택되지 않은 경우, 기본값 지정
      // 필요에 따라 다른 로직으로 대체 가능
      midZoneParam = '0520';
    }

    // 모니터링 서비스에서 알람 계측기 리스트를 가져옴
    this.alarmMeterList = await firstValueFrom(
      this.monitoringSvc.GetAlarmMeterList(startDate, endDate, subZoneParam, midZoneParam)
    );

    // 가져온 데이터로 내부 상태 및 지도 표시 업데이트
    this.updateTotalEventData(this.alarmMeterList);
    this.$mapInitialized.next();
    this.$alarmUpdatedSubject.next(this.alarmMeterList);

    // 성공 토스트 메세지 표시
    this.toastSvc.showToast('작업이 성공적으로 완료되었습니다.', 'success', 3000);
  } catch (error: unknown) {
    // HTTP 예외 처리
    if (error instanceof HttpErrorResponse) {
      if (error.status === 404) {
        this.toastSvc.showToast(
          `데이터를 가져오는 중 오류가 발생했습니다. (${error.error.detail})`,
          'error',
          5000
        );
      } else {
        this.toastSvc.showToast('알 수 없는 오류가 발생했습니다.', 'error', 5000);
      }
    } else {
      this.toastSvc.showToast('예상치 못한 오류가 발생했습니다.', 'error', 5000);
    }
  }
  
 async waitForAlarmMeterList(): Promise<void> {
  try {
    // 이미 alarmMeterList가 존재한다면 재요청할 필요가 없다고 가정
    if (!this.alarmMeterList) {
      let subZoneParam: string | undefined = undefined;
      let midZoneParam: string | undefined = undefined;

      // 1. 소구역 모드 && 소구역 리스트가 존재할 때 -> subZone 파라미터
      if (this.selectedZoneType === 'sub' && this.selectedSubZones.length > 0) {
        subZoneParam = this.selectedSubZones.join(',');
      }
      // 2. 중구역 모드 && 중구역이 선택되어 있을 때 -> midZone 파라미터
      else if (this.selectedZoneType === 'mid' && this.selectedMidZone) {
        midZoneParam = this.selectedMidZone;
      }
      // 3. 아무 구역도 선택되지 않은 경우 -> 기본 중구역(예: '0520') 지정
      else {
        midZoneParam = '0520';
      }

      console.log(
        `알람 데이터를 대기 중:
         startDate=${this.startDate},
         endDate=${this.endDate},
         subZone=${subZoneParam},
         midZone=${midZoneParam}`
      );

      // 모니터링 서비스 호출
      this.alarmMeterList = await firstValueFrom(
        this.monitoringSvc.GetAlarmMeterList(
          this.startDate,
          this.endDate,
          subZoneParam,
          midZoneParam
        )
      );
      // 받은 데이터로 내부 상태 업데이트
      await this.updateTotalEventData(this.alarmMeterList);
      this.$alarmUpdatedSubject.next(this.alarmMeterList);
    }
  } catch (error) {
    console.error('Failed to fetch alarm meter list:', error);
    // 추가 예외 처리 로직
  }
}

/** 1) 알람 목록을 재요청하고 2) 구독자에게 알림을 주는 헬퍼 */
public async refreshAlarmMeterList(): Promise<void> {
  try {
    /* --- 1. 현재 선택 상태 기반 파라미터 구성 -------------------------- */
    let subZoneParam: string | undefined = undefined;
    let midZoneParam: string | undefined = undefined;

    if (this.selectedZoneType === 'sub' && this.selectedSubZones.length > 0) {
      subZoneParam = this.selectedSubZones.join(',');
    } else if (this.selectedZoneType === 'mid' && this.selectedMidZone) {
      midZoneParam = this.selectedMidZone;
    } else {
      midZoneParam = '0520'; 
    }

    /* --- 2. API 재호출 -------------------------------------------------- */
    this.alarmMeterList = await firstValueFrom(
      this.monitoringSvc.GetAlarmMeterList(
        this.startDate,
        this.endDate,
        subZoneParam,
        midZoneParam
      )
    );

    /* --- 3. 통계·UI 업데이트 ------------------------------------------- */
    await this.updateTotalEventData(this.alarmMeterList);

    /* --- 4. 이벤트 발행 ------------------------------------------------- */
    this.$mapInitialized.next();
    this.$alarmUpdatedSubject.next(this.alarmMeterList);

    /* 토스트 메시지 */
    this.toastSvc.showToast('알람 데이터를 새로 불러왔습니다.', 'success', 3000);
  } catch (error) {
    console.error('Failed to refresh alarm meter list:', error);
    this.toastSvc.showToast('알람 데이터 갱신에 실패했습니다.', 'error', 5000);
  } catch (e) {
    this.toastSvc.showToast('알람 갱신 실패', 'error', 5000);
  }
}

// 무엇을 고치려고 하는지, 고치려는 문제가 무엇인지 알려주세요.
// 소스 코드에서 동일한 코드가 반복되고 있고, 이는 잠재적인 버그의 위협을 증가시킵니다.
// 반복되는 코드들을 함수로 묶고 해당 함수를 여러곳에서 호출하는 구조로 변경해야합니다.

  

// After 😎

async loadAlarmMeterList(): Promise<void> {
  let subZoneParam: string | undefined = undefined;
  let midZoneParam: string | undefined = undefined;

  // 1. 소구역 모드 && 소구역 리스트가 존재할 때 -> subZone 파라미터
  if (this.selectedZoneType === 'sub' && this.selectedSubZones.length > 0) {
    subZoneParam = this.selectedSubZones.join(',');
  }
  // 2. 중구역 모드 && 중구역이 선택되어 있을 때 -> midZone 파라미터
  else if (this.selectedZoneType === 'mid' && this.selectedMidZone) {
    midZoneParam = this.selectedMidZone;
  }
  // 3. 아무 구역도 선택되지 않은 경우 -> 기본 중구역(예: '0520') 지정
  else {
    midZoneParam = '0520';
  }

  // 알람 계측기 목록 API 통신 함수 호출
  this.alarmMeterList = await firstValueFrom(
    this.monitoringSvc.GetAlarmMeterList(
      this.startDate, this.endDate, subZoneParam, midZoneParam
    )
  );

  this.$alarmUpdatedSubject.next(this.alarmMeterList);
  
  this.toastSvc.showToast('알람 데이터를 불러왔습니다.', 'success', 3000);
}

// 활용

async updateDate(start: string, end: string): Promise<void> {
  this.startDate = start;
  this.endDate   = end;
  this.$mapDate.next({ startDate: start, endDate: end });

  await this.loadAlarmMeterList(); // 날짜 바뀌면 자동 갱신
}
  
// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// 동일한 로직이 여러 곳에서 중복되면 해당 로직을 수정해야할 때
// 수정되지 않은 공통 로직은 반드시 부수 효과(Side Effect)를 가져온다는 점을 깨달았고
// 중복된 코드는 가능한 줄이는게 좋다는걸 배웠습니다.

```

### QUIZ 03.

```jsx
// 본인이 가장 잘하는 언어로(JS, Python 등등) 더러운 코드를 깨끗한 코드로 
// 리팩토링하는 예시를 만들어보세요. 현재 파일은 JS 로 되어있지만. 자유롭게 다른 언어로 변경해주세요. 

// 원칙 3. 의미 있는 이름

// Before 😣

// 현재 선택된 탭에 따라 적절한 ECharts 옵션을 반환하는 메서드
getChartOptionBySelectedTab(): EChartsOption {
  const tab = this.selectedTab;
  if (tab === 'supplyAI') return this.aiInstantFlowOption;
  if (tab === 'supply') return this.instantFlowOption;
  if (tab === 'supplyCumulative') return this.cumulativeOption;
  return this.instantFlowOption;
}

// 무엇을 고치려고 하는지, 고치려는 문제가 무엇인지 알려주세요.
// 선택된 탭에 따라 적절한 그래프를 보여주기 위해 필요한 함수에서
// 탭의 명칭이 명확하지 않아서 유지보수할 때 코드를 이해하는데 어려움이 걸려 생산성이 떨어지는 문제가 발생

  

// After 😎

// 현재 선택된 탭에 따라 적절한 ECharts 옵션을 반환하는 메서드
getChartOptionBySelectedTab(): EChartsOption {
  const tab = this.selectedTab;
  if (tab === 'AIInstantFlow') return this.AIInstantFlowOption;
  if (tab === 'instantFlow') return this.instantFlowOption;
  if (tab === 'cumulativeFlow') return this.cumulativeOption;
  return this.instantFlowOption;
}
  
// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// 각 탭별로 명확한 명칭을 사용해서 어떤 탭에 어떤 차트 옵션을 반환해야할지 한 눈에 이해할 수 있게 고침
// 의미 있는 이름은 향후 유지보수에 있어서 생산성 향상에 큰 도움이 된다는걸 배웠습니다.
```
