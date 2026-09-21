from machine import Pin, ADC
from neopixel import NeoPixel
import time

# 1. 필수: WS2813 네오픽셀 설정 (유지)
TIMING = (280, 515, 515, 745)
NUM_LEDS = 10  # 사용 중인 LED 개수
led = NeoPixel(Pin(16), NUM_LEDS, timing=TIMING)

sensor = ADC(Pin(26))

while True:
    breath_value = sensor.read_u16()
    print("현재 수치:", breath_value)
    
    # 2. 나만의 값(Threshold)으로 조건 만들기
    # 평소: 약 6600 / 적당함: 약 5200 
    
    if 5000 <= breath_value <= 5500:
        # 적당한 세기 (목표 범위): 전체 초록불
        led.fill((0, 50, 0))
        
    elif breath_value < 5000:
        # 너무 세게 분 경우: 전체 빨간불
        led.fill((50, 0, 0))
        
    elif 5500 < breath_value < 6400:
        # 너무 약하게 분 경우: 전체 파란불 (조금 더 세게 불라는 뜻)
        led.fill((0, 0, 50))
        
    else:
        # 평소 상태 (6400 이상): 불 끄기
        led.fill((0, 0, 0))
        
    led.write()
    time.sleep(0.1)
