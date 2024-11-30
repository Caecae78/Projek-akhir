# Projek-akhir
import psutil
import time
import logging

# Konfigurasi Logging
logging.basicConfig(
    filename="battery_status.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

def get_battery_status():
    battery = psutil.sensors_battery()
    if battery:
        status = {
            "percent": battery.percent,
            "charging": battery.power_plugged,
            "time_left": battery.secsleft if battery.secsleft != psutil.POWER_TIME_UNLIMITED else "Unlimited"
        }
        return status
    else:
        return None

def monitor_battery(interval=60):
    while True:
        status = get_battery_status()
        if status:
            charging_status = "Charging" if status["charging"] else "Not Charging"
            time_left = f"{status['time_left']} seconds" if isinstance(status["time_left"], int) else status["time_left"]
            message = f"Battery: {status['percent']}% | {charging_status} | Time Left: {time_left}"
            
            # Print to Console
            print(message)
            
            # Log to File
            logging.info(message)
        else:
            print("Battery status not available!")
            logging.warning("Battery status not available!")
        
        time.sleep(interval)  # Tunggu sesuai interval

if __name__ == "__main__":
    print("Starting battery monitor...")
    monitor_battery(interval=60)  # Cek setiap 60 detik
