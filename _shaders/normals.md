---
title: "Roughness, normal maps and detail normals"
excerpt: "Bumps without height maps"
date: "2024-05-29"
classes: wide
sidebar:
  - title: "Shader Type"
    text: "HLSL"
  - title: "Additional tags"
    text: "roughness, normal map, detail normal map, bump scale"
header:
  teaser: /assets/images/shaders/hlsl/roughness.png
gallery:
---

Upgraded previous shader to allow for detail maps and their normals, aswell as normal maps and their bumpscales. Example material looks like this:

![Shader](../../assets/images/shaders/hlsl/roughness.png)



CustomLight.cginc
```glsl
#if !defined(MY_LIGHTING_INCLUDED)
#define MY_LIGHTING_INCLUDED
#include "UnityPBSLighting.cginc"
#include "AutoLight.cginc"


float4    _Tint;
sampler2D _MainTex, _DetailTex;
float4    _MainTex_ST, _DetailTex_ST;
float     _Smoothness;
float     _Metallic;
sampler2D _NormalMap, _DetailNormalMap;
float     _BumpScale, _DetailBumpScale;

struct Interpolators {
	float4 position : SV_POSITION;
	float4 uv : TEXCOORD0;
	float3 normal : TEXCOORD1;
	float3 worldpos : TEXCOORD2;

	#if defined(VERTEXLIGHT_ON)
        float3 vertexLightColor : TEXCOORD3;
	#endif
};

struct VertexData {
	float4 position : POSITION;
	float3 normal : NORMAL;
	float2 uv : TEXCOORD0;
};

void ComputeVertexLightColor(inout Interpolators i)
{
	#if defined(VERTEXLIGHT_ON)
        i.vertexLightColor = Shade4PointLights(
            unity_4LightPosX0, unity_4LightPosY0, unity_4LightPosZ0,
            unity_LightColor[0].rgb, unity_LightColor[1].rgb,
            unity_LightColor[2].rgb, unity_LightColor[3].rgb,
            unity_4LightAtten0, i.worldpos, i.normal
        );
	#endif
}

Interpolators Vert(VertexData v)
{
	Interpolators i;
	i.uv.xy = TRANSFORM_TEX(v.uv, _MainTex);
	i.uv.zw = TRANSFORM_TEX(v.uv, _DetailTex);
	i.position = UnityObjectToClipPos(v.position);
	i.worldpos = mul(unity_ObjectToWorld, v.position);
	i.normal = UnityObjectToWorldNormal(v.normal);

	ComputeVertexLightColor(i);

	return i;
}

UnityLight Light(Interpolators i)
{
	UnityLight light;
	#if defined(POINT) || defined(SPOT) || defined(POINT_COOKIE)
        light.dir = normalize(_WorldSpaceLightPos0.xyz - i.worldpos);
	#else
	light.dir = _WorldSpaceLightPos0.xyz;
	#endif

	UNITY_LIGHT_ATTENUATION(attenuation, 0., i.worldpos);
	light.color = _LightColor0.rgb * attenuation;
	light.ndotl = DotClamped(i.normal, light.dir);
	return light;
}

UnityIndirect CreateIndirectLight(Interpolators i)
{
	UnityIndirect indirectLight;
	indirectLight.diffuse = 0;
	indirectLight.specular = 0;

	#if defined(VERTEXLIGHT_ON)
        indirectLight.diffuse = i.vertexLightColor; 
	#endif

	#if defined(FORWARD_BASE_PASS)
	indirectLight.diffuse += max(0, ShadeSH9(float4(i.normal, 1)));
	#endif

	return indirectLight;
}

void InitializeFragmentNormal(inout Interpolators i)
{
	float3 _normal =  UnpackScaleNormal(tex2D(_NormalMap, i.uv.xy), _BumpScale);
	float3 _detailNormal =  UnpackScaleNormal(tex2D(_DetailNormalMap, i.uv.zw), _DetailBumpScale);
	i.normal = (_normal + _detailNormal) * 0.5;
	i.normal = i.normal.xzy;
	i.normal = normalize(i.normal);
}

float4 Frag(Interpolators i) : SV_TARGET
{
	InitializeFragmentNormal(i);
	float3 viewDir = normalize(_WorldSpaceCameraPos - i.worldpos);
	float3 albedo = tex2D(_MainTex, i.uv.xy).rgb * _Tint.rgb;
	albedo *= tex2D(_DetailTex, i.uv.zw) * unity_ColorSpaceDouble;

	float3 specularTint;
	float  oneMinusReflectivity = 1 - _Metallic;
	albedo = DiffuseAndSpecularFromMetallic(albedo, _Metallic, specularTint, oneMinusReflectivity);

	return UNITY_BRDF_PBS(albedo, specularTint, oneMinusReflectivity, _Smoothness, i.normal, viewDir, Light(i), CreateIndirectLight(i));
}


#endif

```

Shader file:
```glsl
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Custom/Roughness Shader"
{
    Properties
    {
        _Tint ("Tint", Color) = (1.,1.,1.,1.)
        _MainTex("Albedo", 2D) = "white" {}
        [NoScaleOffset] _NormalMap ("Normals", 2D) = "bump" {}
        [Gamma] _Metallic("Metallic", Range(0,1)) = 0.
        _BumpScale("Bump Scale", Float) = 1.
        _Smoothness("Smoothness", Range(0,1)) = .5
        _DetailTex("Detail Texture", 2D) = "gray"{}
        [NoScaleOffset] _DetailNormalMap ("Detail Normal Map", 2D) = "bump" {}
        _DetailBumpScale ("Details Bump Scale", Float) = 1. 
    }    
    
    SubShader
    {
        Pass
        {
            Tags {
				"LightMode" = "ForwardBase"
			}           
            
            CGPROGRAM

            #pragma target 3.0
            #pragma vertex Vert
            #pragma fragment Frag
            #pragma multi_compile _ VERTEXLIGHT_ON
            #define FORWARD_BASE_PASS

            #include "CustomLight.cginc"            


            ENDCG
        }
        
        Pass
        {
            Tags {
				"LightMode" = "ForwardAdd"
			}
            
            Blend One One
            Zwrite Off
            
            CGPROGRAM

            #pragma target 3.0
            #pragma vertex Vert
            #pragma fragment Frag
            #pragma multi_compile_fwadd

            #include "CustomLight.cginc"            

            
            
            ENDCG
        } 
    }
}
```
