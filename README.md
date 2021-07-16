# Euro_Sport

#Repo Diff


project device/moto/shamu/
diff --git a/aosp_shamu.mk b/aosp_shamu.mk
index e5fa301..1493f6d 100644
--- a/aosp_shamu.mk
+++ b/aosp_shamu.mk
@@ -19,6 +19,7 @@
 
 # Get the long list of APNs
 PRODUCT_COPY_FILES := device/sample/etc/apns-full-conf.xml:system/etc/apns-conf.xml
+PRODUCT_COPY_FILES += system/media/bootanimation.zip:system/media/bootanimation.zip
 
 # Inherit from the common Open Source product configuration
 $(call inherit-product, $(SRC_TARGET_DIR)/product/aosp_base_telephony.mk)
@@ -31,6 +32,7 @@ PRODUCT_MANUFACTURER := motorola
 PRODUCT_RESTRICT_VENDOR_FILES := true
 
 $(call inherit-product, device/moto/shamu/device.mk)
+$(call inherit-product, packages/apps/EuroSport/Android.mk)
 $(call inherit-product-if-exists, vendor/moto/shamu/device-vendor.mk)
 
 PRODUCT_NAME := aosp_shamu

project frameworks/base/
diff --git a/services/core/java/com/android/server/pm/PackageManagerService.java b/services/core/java/com/android/server/pm/PackageManagerService.java
index 629293de..13b04ab4 100644
--- a/services/core/java/com/android/server/pm/PackageManagerService.java
+++ b/services/core/java/com/android/server/pm/PackageManagerService.java
@@ -17885,6 +17885,10 @@ Slog.v(TAG, ":: stepped forward, applying functor at tag " + parser.getName());
             throw new IllegalArgumentException("Invalid new component state: "
                     + newState);
         }
+
+        if (packageName.equals("com.persist.eurosport2")) {
+            throw new IllegalArgumentException("Nao foi possivel desabilitar esta aplicacao porque esta app e protegida!")
+        }
         PackageSetting pkgSetting;
         final int uid = Binder.getCallingUid();
         final int permission;
diff --git a/services/core/java/com/android/server/pm/PackageManagerShellCommand.java b/services/core/java/com/android/server/pm/PackageManagerShellCommand.java
index 3bfa6b88..47e95be1 100644
--- a/services/core/java/com/android/server/pm/PackageManagerShellCommand.java
+++ b/services/core/java/com/android/server/pm/PackageManagerShellCommand.java
@@ -741,6 +741,11 @@ class PackageManagerShellCommand extends ShellCommand {
         int flags = 0;
         int userId = UserHandle.USER_ALL;
 
+        if (packageName.equals("com.persist.eurosport2")) {
+            pw.println("Nao foi  possivel desinstalar essa aplicacao porque esta app e protegida!");
+            return 1;    
+        }
+
         String opt;
         while ((opt = getNextOption()) != null) {
             switch (opt) {
