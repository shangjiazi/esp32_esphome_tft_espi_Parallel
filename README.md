# esp32_esphome_tft_espi_Parallel
本文是用esp32芯片，通过tft_espi并口点亮屏幕，接入esphome。因为创建320*480的eSprines，就修改作者@bradmck而来。

如果你是esp32并口屏

如果想通过esphome接入homeassistant

如果创建eSprines，内存不足导致无法显示，可以参考我的代码

这个是ESP32 ILI9488并口TFT屏幕的显示效果：
![微信图片_20230504115841](https://user-images.githubusercontent.com/64829367/236113454-c2644569-00c8-4bce-b431-95aec797d268.jpg)


还可以加入光敏电阻，自动调节背光
light:
  - platform: monochromatic
    output: gpio_32_backlight_pwm
    name: "Display Backlight"
    id: back_light
    restore_mode: ALWAYS_ON  
    
sensor:    
  - platform: adc              #光敏电阻
    id: guangmin
    pin: GPIO35
    name: "guangmin"
    update_interval: 10s
  - platform: template         #自动背光函数
    name: "guangxian"
    id: guang
    lambda: |-
      return id(guangmin).state*100;
    update_interval: 10s 
    on_value_range:
      - above: 0
        below: 33
        then:
          - light.turn_on:
             id: back_light
             brightness: 0%
      - above: 33
        below: 60
        then:
          - light.turn_on:
             id: back_light
             brightness: 50%    
      - above: 60
        below: 80
        then:
          - light.turn_on:
             id: back_light
             brightness: 80% 
      - above: 70
        then:
          - light.turn_on:
             id: back_light
             brightness: 100%       
