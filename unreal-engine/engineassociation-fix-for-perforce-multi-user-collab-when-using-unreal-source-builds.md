# EngineAssociation Fix for perforce multi-user collab when using Unreal source builds

{% hint style="info" %}
EngineAssociation - is a property inside Unreal Project's ".uproject" file, which defines which version of unreal engine does a particular unreal project use.
{% endhint %}

#### The Issue

For Unreal Binary builds (unreal engine versions that are downloaded through Epic games launcher) the EngineAssociation property is set to version number like "5.1.1". When you push this project to perforce and when other tries to access it, as long as they have same engine version installed the project opens perfectly.&#x20;

But thats not the case with Unreal Source build (Unreal engine versions generated by  taking source code from github), as source builds cannot just be identified by version as users with source build usually tend to tweak the engine code according to their needs. So EngineAssociation for projects using unreal source builds are set to a random string which maps to a File Path in windows registry. So everytime you open that project, Unreal would take that random string and look for an entry with that value in windows registry, if it finds an entry it retrives the file path to the unreal source build exe from it and uses it to open that project.

The issue here is, if your unreal source is added into a subfolder of your project, it works fine when shared over perforce, as it doesnt store engine association any more as it is included in the project., if not when you share it in perforce - everytime other users try to open it, it rebuilds for their engine as engine associations recorded for other users can be different (as Random string associated to their local source build path can be different for everyuser), Causing all other users to rebuild everytime they get a new version of the project.



#### The Solution

Solution is to use <mark style="color:yellow;">same Random string for EngineAssociation</mark> value in uproject file for all the users. And also <mark style="color:yellow;">update windows registry key</mark> in all user computers to use the new random string, so that unreal finds the source build location in your computer from the same random string used by other.



#### Path to unreal source build association registry keys

```
HKEY_CURRENT_USER\SOFTWARE\Epic Games\UnrealEngine\Builds
```



#### References

* [https://docs.unrealengine.com/5.0/en-US/using-an-installed-build-of-unreal-engine/](https://docs.unrealengine.com/5.0/en-US/using-an-installed-build-of-unreal-engine/)
* [https://docs.unrealengine.com/4.26/en-US/API/Runtime/Projects/FProjectDescriptor/EngineAssociation/](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Projects/FProjectDescriptor/EngineAssociation/)