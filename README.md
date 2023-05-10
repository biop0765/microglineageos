# A lineageos archive

A repo for LOS archive

## Releases

See releases for images of various devices

## To upload

```bash
#go install github.com/github-release/github-release@latest

# Create a tag
github-release release  --user biop0765 --repo assorted_lineageos_backup --tag oriole

download_microg_images () {
myArray=("taimen" "blueline" "crosshatch" "sargo" "bonito" "flame" "coral" "sunfish" "bramble" "redfin" "barbet" "marlin")
for LIST_OF_DEVICES in ${myArray[@]}
do
 wget --wait 10 --random-wait \
      --no-clobber \
      -r -l1 --no-parent --no-directories \
      --no-verbose https://download.lineage.microg.org/$LIST_OF_DEVICES/
 rm -f index.*
done
}
GITHUB_TOKEN=
GH_USERNAME=
GH_REPO=
download_google_factory_images () {
curl -L https://gist.github.com/biop0765/eb4c302078baa83526fd45b2aa49a15e/raw -o url_pixel_factory_images.txt
for i in `cat url_pixel_factory_images.txt`
  do
  curl -O $i
  sleep $(( ( RANDOM % 120)  + 1))
  done
}
myArray=("taimen" "blueline" "crosshatch" "sargo" "bonito" "flame" "coral" "sunfish" "bramble" "redfin" "barbet" "marlin")
for GH_TAG in ${myArray[@]}
do
   rm -f list_of_current_artifacts
   touch list_of_current_artifacts
   github-release info --user $GH_USERNAME --repo $GH_REPO | grep artifact | grep $GH_TAG | grep artifact | awk '{print $3}' > list_of_current_artifacts
   #find . -type f \( -iname *$GH_TAG*\*.zip -o -iname *$GH_TAG*\*.sha256sum -o -iname *$GH_TAG*\*recovery*.img \) -printf '%P\n'
   #wget -r -l1 --no-parent --no-directories --no-verbose https://download.lineage.microg.org/$GH_TAG/
   for i in `find . -type f \( -iname *$GH_TAG*\*.zip -o -iname *$GH_TAG*\*.sha256sum -o -iname *$GH_TAG*\*recovery*.img \) -printf '%P\n'`
   #for i in `find . -type f \( -iname *$GH_TAG*-ota-\*.zip \) -printf '%P\n'`
     do
       if grep $i list_of_current_artifacts > /dev/null ; then
         echo $i "exists in github"
       else
         #Create the release if needed
         #github-release release --user $GH_USERNAME --repo $GH_REPO --tag $GH_TAG
         echo "Uploading " $i " to github"
         github-release upload --user $GH_USERNAME \
           --repo $GH_REPO \
           --tag  $GH_TAG  \
           --name  $i --file  $i
         echo "Sleeping"
         sleep $(( ( RANDOM % 120)  + 1))
         #rm -f $i
       fi
   done
done
```

## 
