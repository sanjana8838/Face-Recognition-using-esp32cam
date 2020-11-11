#define CAMERA_MODEL_WROVER_KIT
#define CAMERA_MODEL_AI_THINKER

#include "esp_camera.h"
#include "model.h"
#include "camerapins.h"

#define FRAME_SIZE FRAMESIZE_QQVGA
#define WIDTH 160
#define HEIGHT 120
#define BLOCK_SIZE 10
#define W (WIDTH / BLOCK_SIZE)
#define H (HEIGHT / BLOCK_SIZE)
Eloquent::ML::Port::SVM pr;
uint16_t prev_frame[H][W] = { 0 };
uint16_t current_frame[H][W] = { 0 };
uint16_t rgb_frame[H][W] = { 0 };
float features[H*W] = { 0 };

bool capture_still();
void convert_to_rbg(uint8_t*, size_t);
void linearize_features();
void classify();

void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(true);
  Serial.println();

//CAMERA SETUP
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;
  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;
  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;
  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  //init with high specs to pre-allocate larger buffers
  if(psramFound()){
    config.frame_size = FRAMESIZE_UXGA;
    config.jpeg_quality = 10;
    config.fb_count = 2;
  } else {
    config.frame_size = FRAMESIZE_SVGA;
    config.jpeg_quality = 12;
    config.fb_count = 1;
  }

#if defined(CAMERA_MODEL_ESP_EYE)
  pinMode(13, INPUT_PULLUP);
  pinMode(14, INPUT_PULLUP);
#endif

//CAMERA INIT
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed with error 0x%x", err);
    return;
  }

  sensor_t * s = esp_camera_sensor_get();
  //initial sensors are flipped vertically and colors are a bit saturated
  if (s->id.PID == OV3660_PID) {
    s->set_vflip(s, 1); //flip it back
    s->set_brightness(s, 1); //increase the brightness 
    s->set_saturation(s, -2); //lower the saturation
  }
  //drop down frame size for higher initial frame rate
  s->set_framesize(s, FRAMESIZE_QVGA);

#if defined(CAMERA_MODEL_M5STACK_WIDE)
  s->set_vflip(s, 1);
  s->set_hmirror(s, 1);
#endif

  
}

void loop() {
  if (!capture_still()) {
        Serial.println("Failed capture");
        delay(2000);

        return;
    }

    linearize_features();
    classify();
    delay(10000);
}

//FUNCTION TO CAPTURE IMAGE
bool capture_still() {
    camera_fb_t *frame_buffer = esp_camera_fb_get();

    if (!frame_buffer)
        return false;

    // set all 0s in current frame
    for (int y = 0; y < H; y++)
       { 
        for (int x = 0; x < W; x++){
          current_frame[y][x] = 0;
       }
       }    
    // down-sample image in blocks
    for (uint32_t i = 0; i < WIDTH * HEIGHT; i++) {
        const uint16_t x = i % WIDTH;
        const uint16_t y = floor(i / WIDTH);
        const uint8_t block_x = floor(x / BLOCK_SIZE);
        const uint8_t block_y = floor(y / BLOCK_SIZE);
        const uint8_t pixel = frame_buffer->buf[i];
        const uint16_t current = current_frame[block_y][block_x];

        // average pixels in block (accumulate)
        current_frame[block_y][block_x] += pixel;
    }
    // average pixels in block (rescale)
    for (int y = 0; y < H; y++)
        {
          for (int x = 0; x < W; x++){
            current_frame[y][x] /= BLOCK_SIZE * BLOCK_SIZE;
        }
      }
}

//FUNCTION TO LINEARIZE CAPTURED IMAGE
void linearize_features() {
    size_t i = 0;

    for (int y = 0; y < H; y++) {
        for (int x = 0; x < W; x++) {
            features[i++] = current_frame[y][x];
        }
    }
}

void classify() {
    Serial.println(pr.predict(features));

   #define CAMERA_MODEL_WROVER_KIT
#define CAMERA_MODEL_AI_THINKER

#include "esp_camera.h"
#include "model.h"
#include "camerapins.h"

// Select camera model

//#define CAMERA_MODEL_ESP_EYE
//#define CAMERA_MODEL_M5STACK_PSRAM
//#define CAMERA_MODEL_M5STACK_WIDE



#define FRAME_SIZE FRAMESIZE_QQVGA
#define WIDTH 160
#define HEIGHT 120
#define BLOCK_SIZE 10
#define W (WIDTH / BLOCK_SIZE)
#define H (HEIGHT / BLOCK_SIZE)
Eloquent::ML::Port::SVM pr;
uint16_t prev_frame[H][W] = { 0 };
uint16_t current_frame[H][W] = { 0 };
uint16_t rgb_frame[H][W] = { 0 };
float features[H*W] = { 0 };

bool capture_still();
void convert_to_rbg(uint8_t*, size_t);
void linearize_features();
void classify();

void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(true);
  Serial.println();

//CAMERA SETUP
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;
  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;
  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;
  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  //init with high specs to pre-allocate larger buffers
  if(psramFound()){
    config.frame_size = FRAMESIZE_UXGA;
    config.jpeg_quality = 10;
    config.fb_count = 2;
  } else {
    config.frame_size = FRAMESIZE_SVGA;
    config.jpeg_quality = 12;
    config.fb_count = 1;
  }

#if defined(CAMERA_MODEL_ESP_EYE)
  pinMode(13, INPUT_PULLUP);
  pinMode(14, INPUT_PULLUP);
#endif

  // camera init
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed with error 0x%x", err);
    return;
  }

  sensor_t * s = esp_camera_sensor_get();
  //initial sensors are flipped vertically and colors are a bit saturated
  if (s->id.PID == OV3660_PID) {
    s->set_vflip(s, 1);//flip it back
    s->set_brightness(s, 1);//up the blightness just a bit
    s->set_saturation(s, -2);//lower the saturation
  }
  //drop down frame size for higher initial frame rate
  s->set_framesize(s, FRAMESIZE_QVGA);

#if defined(CAMERA_MODEL_M5STACK_WIDE)
  s->set_vflip(s, 1);
  s->set_hmirror(s, 1);
#endif

  
}

void loop() {
  if (!capture_still()) {
        Serial.println("Failed capture");
        delay(2000);

        return;
    }

    linearize_features();
    classify();
    delay(10000);
}

bool capture_still() {
    camera_fb_t *frame_buffer = esp_camera_fb_get();

    if (!frame_buffer)
        return false;

    // set all 0s in current frame
    for (int y = 0; y < H; y++)
       { 
        for (int x = 0; x < W; x++){
          current_frame[y][x] = 0;
       }
       }    
    // down-sample image in blocks
    for (uint32_t i = 0; i < WIDTH * HEIGHT; i++) {
        const uint16_t x = i % WIDTH;
        const uint16_t y = floor(i / WIDTH);
        const uint8_t block_x = floor(x / BLOCK_SIZE);
        const uint8_t block_y = floor(y / BLOCK_SIZE);
        const uint8_t pixel = frame_buffer->buf[i];
        const uint16_t current = current_frame[block_y][block_x];

        // average pixels in block (accumulate)
        current_frame[block_y][block_x] += pixel;
    }
    // average pixels in block (rescale)
    for (int y = 0; y < H; y++)
        {
          for (int x = 0; x < W; x++){
            current_frame[y][x] /= BLOCK_SIZE * BLOCK_SIZE;
        }
      }
}

void linearize_features() {
    size_t i = 0;

    for (int y = 0; y < H; y++) {
        for (int x = 0; x < W; x++) {
            features[i++] = current_frame[y][x];
        }
    }
}

void classify() {
    Serial.println(pr.predict(features));
    if(pr.predict(features))
      Serial.println("1");
    else
      Serial.println("0");
    
}
