# 检测并删除材质中记录的冗余Keywords和Shader属性
```C#
#region classname  
/*  
 *         Title: classname : Ufw *         Description: *                功能：       检测并删除材质中记录的冗余Keywords和Shader属性  
 *         Author:            陈卓维  
 *         Time:              2022年9月13日11:21:27  
 *         Version:           0.1版本  
 *         Modify Recorde: */  
#endregion  
using System;  
using System.Collections.Generic;  
using System.Reflection;  
using UnityEditor;  
using UnityEngine;  
  
namespace Ufw  
{  
    public class UnusedKeywordsAndPropertiesCheckerEditor : Editor  
    {  
        //获取shader中所有的宏  
        private static bool GetShaderKeywords(Shader target, out string[] global, out string[] local)  
        {  
            try  
            {  
                MethodInfo globalKeywords = typeof(ShaderUtil).GetMethod("GetShaderGlobalKeywords", BindingFlags.Static | BindingFlags.Public | BindingFlags.NonPublic);  
                MethodInfo localKeywords = typeof(ShaderUtil).GetMethod("GetShaderLocalKeywords", BindingFlags.Static | BindingFlags.Public | BindingFlags.NonPublic);  
                if (globalKeywords != null)  
                {  
                    global = (string[])globalKeywords.Invoke(null, new object[]  
                    {                        target  
                    });  
                }  
                else  
                {  
                    global = null;  
                }  
                if (localKeywords != null)  
                {  
                    local = (string[])localKeywords.Invoke(null, new object[]  
                    {                        target  
                    });  
                }  
                else  
                {  
                    local = null;  
                }  
                return true;  
            }  
            catch (Exception e)  
            {  
                Log.Error($"获取shader中所有的宏,错误: {e.Message}");  
                global = local = null;  
                return false;  
            }  
        }  
  
        [MenuItem(UfwMenuItemIni.M_Check_MatKey)]  
        public static void CheckMaterials()  
        {  
            EditorUtility.ClearProgressBar();  
            ProfilerUtils.BeginWatch("CheckMaterials");  
            string[] paths = AssetDatabase.FindAssets("t:material", new[]  
            {  
                "Assets/Res"  
            });  
            if (paths == null || paths.Length == 0)  
            {  
                Log.Info($"未找到相匹配的文件 匹配格式: t:material 匹配路径: Assets/Res");  
                return;  
            }  
            int index = 0;  
            int maxCount = paths.Length;  
            foreach (string materialGuid in paths)  
            {  
                string materialAssetPath = AssetDatabase.GUIDToAssetPath(materialGuid);  
                Material material = AssetDatabase.LoadAssetAtPath<Material>(materialAssetPath);  
                EditorUtility.DisplayProgressBar($"读取文件Material文件: {material.name}", $"正在查询内容", (float)index / maxCount);  
                if (GetShaderKeywords(material.shader, out string[] global, out string[] local))  
                {  
                    HashSet<string> keywords = new HashSet<string>();  
                    if (global != null)  
                    {                        foreach (string g in global)  
                        {  
                            keywords.Add(g);  
                        }  
                    }  
                    if (local != null)  
                    {                        foreach (string l in local)  
                        {  
                            keywords.Add(l);  
                        }  
                    }  
                    if (keywords.Count == 0)  
                    {                        Log.Warn($"读取文件失效,请检查Material: {material.name}  Shader: {material.shader}");  
                        continue;  
                    }  
                    EditorUtility.ClearProgressBar();  
  
                    //重置keywords  
                    List<string> resetKeywords = new List<string>(material.shaderKeywords);  
                    int removeIndex = 0;  
                    foreach (string item in material.shaderKeywords)  
                    {                        EditorUtility.DisplayProgressBar($"检查KeyWords: {material.name}", $"匹配字段 {item}", (float)removeIndex / resetKeywords.Count);  
                        if (!keywords.Contains(item))  
                        {  
                            resetKeywords.Remove(item);  
                        }  
                        removeIndex++;  
                    }  
                    material.shaderKeywords = resetKeywords.ToArray();  
                }  
                else  
                {  
                    Log.Warn($"获取shader中所有的宏,Error: Material:{material.name}  shader:{material.shader}");  
                    continue;  
                }  
                EditorUtility.ClearProgressBar();  
                HashSet<string> property = new HashSet<string>();  
                int count = material.shader.GetPropertyCount();  
                for (int i = 0; i < count; i++)  
                {  
                    property.Add(material.shader.GetPropertyName(i));  
                }  
                SerializedObject o = new SerializedObject(material);  
                SerializedProperty disabledShaderPasses = o.FindProperty("disabledShaderPasses");  
                SerializedProperty savedProperties = o.FindProperty("m_SavedProperties");  
                SerializedProperty texEnvs = savedProperties.FindPropertyRelative("m_TexEnvs");  
                SerializedProperty floats = savedProperties.FindPropertyRelative("m_Floats");  
                SerializedProperty colors = savedProperties.FindPropertyRelative("m_Colors");  
                int removeAttrId = 0;  
                int removeAttrCount = disabledShaderPasses.arraySize + texEnvs.arraySize + floats.arraySize + colors.arraySize;  
                EditorUtility.ClearProgressBar();  
                //对比属性删除残留的属性  
                for (int i = disabledShaderPasses.arraySize - 1; i >= 0; i--)  
                {  
                    string displayName = disabledShaderPasses.GetArrayElementAtIndex(i).displayName;  
                    EditorUtility.DisplayProgressBar($"{material.name} 对比:disabledShaderPasses", $"检查属性{displayName}", (float)removeAttrId / removeAttrCount);  
                    if (!property.Contains(displayName))  
                    {                        disabledShaderPasses.DeleteArrayElementAtIndex(i);  
                    }  
                    removeAttrId++;  
                }  
                EditorUtility.ClearProgressBar();  
                for (int i = texEnvs.arraySize - 1; i >= 0; i--)  
                {  
                    string displayName = texEnvs.GetArrayElementAtIndex(i).displayName;  
                    EditorUtility.DisplayProgressBar($"{material.name} 对比:m_TexEnvs", $"检查属性{displayName}", (float)removeAttrId / removeAttrCount);  
                    if (!property.Contains(displayName))  
                    {                        texEnvs.DeleteArrayElementAtIndex(i);  
                    }  
                    removeAttrId++;  
                }  
                EditorUtility.ClearProgressBar();  
                for (int i = floats.arraySize - 1; i >= 0; i--)  
                {  
                    string displayName = floats.GetArrayElementAtIndex(i).displayName;  
                    EditorUtility.DisplayProgressBar($"{material.name} 对比:m_Floats", $"检查属性{displayName}", (float)removeAttrId / removeAttrCount);  
                    if (!property.Contains(displayName))  
                    {                        floats.DeleteArrayElementAtIndex(i);  
                    }  
                    removeAttrId++;  
                }  
                EditorUtility.ClearProgressBar();  
                for (int i = colors.arraySize - 1; i >= 0; i--)  
                {  
                    string displayName = colors.GetArrayElementAtIndex(i).displayName;  
                    EditorUtility.DisplayProgressBar($"{material.name} 对比:m_Colors", $"检查属性{displayName}", (float)removeAttrId / removeAttrCount);  
                    if (!property.Contains(displayName))  
                    {                        colors.DeleteArrayElementAtIndex(i);  
                    }  
                    removeAttrId++;  
                }  
                o.ApplyModifiedProperties();  
                index += 1;  
            }  
            UWatchResult result = ProfilerUtils.EndWatch("CheckMaterials");  
            Log.Info($"清理Material残留 时间{result.costTime} [内存]{result.costMemory}");  
            EditorUtility.ClearProgressBar();  
            AssetDatabase.SaveAssets();  
            Log.Info("检查完毕");  
        }  
    }  
}
```