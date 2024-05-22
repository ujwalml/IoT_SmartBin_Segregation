# IoT_SmartBin_Segregation
This project helps us to segregate wet waste and dry waste. We have used many sensors such as IR, Ultrasonic sensors. Microcontroller used here is Esp32, we also used servomotor to segregate the wet and dry waste into its respective bins.
from machine import Pin, PWM
import time
import machine
from machine import Pin, I2C
from lcd_api import LcdApi
from i2c_lcd import I2cLcd
from time import sleep

# Define pin numbers
I2C_ADDR = 0x27
totalRows = 2
totalColumns = 16
#servo_pin = 23  # Change this to the GPIO pin you are using

i2c = I2C(scl=Pin(22), sda=Pin(21), freq=10000)
ir_sensor_pin = 14
moisture_sensor_pin = 16
lcd = I2cLcd(i2c, I2C_ADDR, totalRows, totalColumns)
# Initialize IR sensor
ir_sensor = Pin(ir_sensor_pin, Pin.IN)

# Initialize moisture sensor
moisture_sensor = Pin(moisture_sensor_pin, Pin.IN)

# Initialize servo motor
servo1 = PWM(Pin(15), freq=50, duty=77)
servo2 = PWM(Pin(23), freq=50, duty=77)
def mov():
    servo2.duty(40)
    time.sleep(0.3)
    servo2.duty(80)
def segregate_waste():
    # Check IR sensor value to determine waste type
    if ir_sensor.value() == 0:
        print("Waste detected")
        # Check moisture sensor for wet waste confirmation
        if moisture_sensor.value() == 0:
            print("wet waste")
            lcd.putstr("WET WASTE")
            segregate_wet_waste()
            sleep(2)
            lcd.clear()
        else:
            print("dry waste")
            lcd.putstr("DRY WASTE")
            segregate_dry_waste()
            sleep(2)
            lcd.clear()
    else:
        print("NOTHING")
        seg()
        lcd.putstr("NO WASTE")
        sleep(2)
        lcd.clear()
def seg():
    servo1.duty(0)
def segregate_wet_waste():
    # Perform actions for wet waste (e.g., activate servo to move to wet waste bin)
    servo1.duty(50)
    time.sleep(1)
    servo1.duty(80)

def segregate_dry_waste():
    # Perform actions for dry waste (e.g., activate servo to move to dry waste bin)
    servo1.duty(110)
    time.sleep(1)
    servo1.duty(77)


lcd.putstr("Hello!")
sleep(2)
lcd.clear()
lcd.putstr("IOT SMART BIN")
sleep(2)
lcd.clear()
while True:
    # Read the digital signal from the soil moisture sensor
    lcd.putstr("ENTER WASTE")
    sleep(2)
    lcd.clear()
    mov()
    segregate_waste()
