# local_manifests — OnePlus 12R (aston) / Ace 3 (astonc)

Drop-in local manifests for **Infinity-X 17** (Android 17).

```bash
mkdir -p .repo/local_manifests
cp aston.xml .repo/local_manifests/     # OnePlus 12R
# or
cp astonc.xml .repo/local_manifests/    # OnePlus Ace 3
repo sync -c --force-sync --no-clone-bundle --no-tags -j16
```

## What gets cloned

| path | repo | branch |
|---|---|---|
| device/oneplus/aston *(or astonc)* | EXlipsecore/android_device_oneplus_aston | `17` |
| device/oneplus/sm8550-common | EXlipsecore/android_device_oneplus_sm8550-common | `17` |
| kernel/oneplus/sm8550 | EXlipsecore/android_kernel_oneplus_sm8550 | `17` |
| kernel/oneplus/sm8550-modules | EXlipsecore/android_kernel_oneplus_sm8550-modules | `17` |
| kernel/oneplus/sm8550-devicetrees | EXlipsecore/android_kernel_oneplus_sm8550-devicetrees | `17` |
| hardware/oplus | EXlipsecore/android_hardware_oplus | `17` |
| hardware/pixelworks/interfaces | LineageOS/android_hardware_pixelworks_interfaces | `lineage-24.0` |
| device/qcom/sepolicy_vndr/sm8550 | EXlipsecore/android_device_qcom_sepolicy_vndr | `17` |
| vendor/oneplus/aston *(or astonc)* | EXlipsecore/proprietary_vendor_oneplus_aston *(GitLab)* | `17` |
| vendor/oneplus/sm8550-common | EXlipsecore/proprietary_vendor_oneplus_sm8550-common *(GitLab)* | `17` |
| vendor/oplus/camera | EXlipsecore/proprietary_vendor_oplus_camera *(GitLab)* | `17` |
| vendor/oneplus/ir | alphadroid-project/vendor_oneplus_ir *(GitLab)* | `alpha-16.2` |
| vendor/oneplus/dolby | crDroidAndroid/proprietary_vendor_oneplus_dolby | `17.0` |
| packages/apps/LunarisDolby | crDroidAndroid/android_packages_apps_LunarisDolby | `17.0` |

