#include <iostream>
#include <cmath>
#include <cstdlib>

#include "uiuc/PNG.h"
#include "uiuc/HSLAPixel.h"
#include "ImageTransform.h"



using uiuc::PNG;
using uiuc::HSLAPixel;

PNG grayscale(PNG image) {
  /// This function is already written for you so you can see how to
  /// interact with our PNG class.
  for (unsigned x = 0; x < image.width(); x++) {
    for (unsigned y = 0; y < image.height(); y++) {
      HSLAPixel & pixel = image.getPixel(x, y);

      // `pixel` is a reference to the memory stored inside of the PNG `image`,
      // which means you're changing the image directly. No need to `set`
      // the pixel since you're directly changing the memory of the image.
      pixel.s = 0;
    }
  }

  return image;
}




PNG createSpotlight(PNG image, int centerX, int centerY) {
  unsigned newx = static_cast<unsigned>(centerX);
  unsigned newy = static_cast<unsigned>(centerY);

  for (unsigned x = 0; x < image.width(); ++x) {
    for (unsigned y = 0; y < image.height(); ++y) {
      if(x == newx && y == newy) {
        continue;
      }

      HSLAPixel & pixel = image.getPixel(x, y);
      double lum = std::sqrt((newx-x)*(newx-x) + (newy-y)*(newy-y))*static_cast<double>(0.005);

      if (lum >= static_cast<double>(0.8)) {
        lum = static_cast<double>(0.8);
      }
      lum = 1.0 - lum;
      pixel.l =  (pixel.l * lum);
    }// y
  }// x

  return image;

}



PNG illinify(PNG image) {
  for (unsigned x = 0; x < image.width(); ++x) {
    for (unsigned y = 0; y < image.height(); ++y) {
      HSLAPixel & pixel = image.getPixel(x, y);
      if( pixel.h >= 200 && pixel.h < 330) {
        pixel.h = 216;
      }
      else {
        pixel.h = 11;
      }
    }
  }
  return image;
}



PNG watermark(PNG firstImage, PNG secondImage) {
  for (unsigned x = 0; x < secondImage.width(); x++) {
    for (unsigned y = 0; y < secondImage.height(); y++) {
      HSLAPixel& firstPixel = firstImage.getPixel(x, y);
      HSLAPixel& secondPixel = secondImage.getPixel(x, y);
      if(secondPixel.l == 1.0) {
        firstPixel.l += 0.2;
      }
    }
  }
  return firstImage;
}
