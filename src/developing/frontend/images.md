# Images

## Optimizing images

This script only works on Linux (or WSL). 

### Dependencies
- img-optimize - https://virtubox.github.io/img-optimize/ (`optimize.sh`)
- imagemagick - https://imagemagick.org/script/download.php (`convert`)


- jpegoptim
- optipng
- cwebp

The last 3 can be installed on Debian/Ubuntu using:

```
sudo apt install jpegoptim optipng webp
```


Once you've downloaded the first script, run the following script from the img-optimize main folder (be sure to replace `<DSAV-Dodeka repository location>` by the correct path):

```bash
#!/bin/bash
# Script by https://christitus.com/script-for-optimizing-images/ (Chris Titus)
# Modified by Tip ten Brink

FOLDER="<DSAV-Dodeka repository location>/src/images"

#resize png or jpg to either height or width, keeps proportions using imagemagick
find ${FOLDER} -iname '*.jpg' -o -iname '*.png' -exec convert \{} -verbose -resize 2400x\> \{} \;
find ${FOLDER} -iname '*.jpg' -o -iname '*.png' -exec convert \{} -verbose -resize x1300\> \{} \;
find ${FOLDER} -iname '*.png' -exec convert \{} -verbose -resize 2400x\> \{} \;
find ${FOLDER} -iname '*.png' -exec convert \{} -verbose -resize x1300\> \{} \;
# Optimize.sh is the img-optimize script
./optimize.sh --std --path ${FOLDER}
```

We convert the images to a size of max 2400x1300, as higher resolutions don't make a big difference.